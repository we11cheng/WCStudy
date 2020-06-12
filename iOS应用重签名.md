#### 应用重签名(python3)

#### python文件源码
```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import zipfile
import os.path
import os
import time
import shutil
import subprocess
import plistlib
import sys,getopt

signextensions      = ['.framework/','.dylib','.appex/','.app/']
bundleidentifierkey = 'CFBundleIdentifier'
replaceplistkey     = 'BundleIdentifier'
oldbundleId         = None 
uncheckedfiles      = [] #暂未检查bundleId文件列表
certificatelist     = [] #证书列表
executableName      = None
version             = 'v1.0.0'

#用户参数
zipFilePath         = None
outputPath          = None
certificate         = None
mobileprovision     = None
entilement          = None
newBundleIdentifier = None

appexs = {}

#拷贝mobileprovsion到xxx.app目录
def copyprovsion2appdir(originpath,mobileprovision):
	for dirpath, dirnames, filenames in os.walk(originpath):
		if dirpath[dirpath.rfind('.'):] == '.app':
			shutil.copy(mobileprovision,'%s/%s' % (dirpath,'embedded.mobileprovision'))
			return True
	return False

#根据mobileprovision生成entitlements.plist文件
def generateentitlements(mobileprovisionpath,entilementspath):
	entilementfull = entilementspath[:entilementspath.rfind('.')] + '_full.plist'
	(status1, output1) = subprocess.getstatusoutput('security cms -D -i "%s" > %s' % (mobileprovisionpath, entilementfull))
	(status2, output2) = subprocess.getstatusoutput('/usr/libexec/PlistBuddy -x -c "Print:Entitlements" %s > %s' % (entilementfull,entilementspath))
	# return status1 == 0 and status2 == 0
	#new for wechat ?
	# with open(entilementspath, 'rb') as fp:
	# 	pl = plistlib.load(fp)
	# 	print(pl)
	# 	time.sleep(10)

	# 	# bid = 
	# 	pl["com.apple.security.application-groups"] = ["group.com.Longzu.gadiao02"]#尝试手动添加权限  安装报错。。
	# 	print(pl)
	# 	time.sleep(10)

	# 	with open(entilementspath, 'wb') as fp:
	# 		plistlib.dump(pl, fp)
	return status1 == 0 and status2 == 0


#修改BundleIdentifier
def modifyBundleIdentifer(originpath,newBundleIdentifier):
	for dirpath,dirnames, filenames in os.walk(originpath):
		for filename in filenames:
			if os.path.split(filename)[-1] == 'Info.plist':
				modifyPlistBundleId(os.path.join(dirpath, filename),newBundleIdentifier)
	for filepath in uncheckedfiles:
		modifyPlistBundleId(filepath,newBundleIdentifier)

#修改Plist文件
def modifyPlistBundleId(filepath,newBundleIdentifier):
	with open(filepath, 'rb') as fp:
		pl = plistlib.load(fp)
		global oldbundleId
		if oldbundleId == None:
			oldbundleId = pl.get(bundleidentifierkey)
		if oldbundleId == None:
			uncheckedfiles.append(filepath)
			return
		for key in pl.keys():
			if replaceplistkey in key:
				pl[key] = pl[key].replace(oldbundleId,newBundleIdentifier)
			elif key == 'NSExtension' and 'NSExtensionAttributes' in pl['NSExtension'] and 'WKAppBundleIdentifier' in pl['NSExtension']['NSExtensionAttributes']:
				extAtts = pl['NSExtension']['NSExtensionAttributes']
				extAtts['WKAppBundleIdentifier'] = extAtts['WKAppBundleIdentifier'].replace(oldbundleId,newBundleIdentifier)
		with open(filepath, 'wb') as fp:
			plistlib.dump(pl, fp)

#获取证书列表
def getCertificates():
	try:
		(status,output) = subprocess.getstatusoutput('security find-identity -v -p codesigning')
		print(' 序号\t\t\tSHA-1\t\t\t证书名称')
		global certificatelist
		certificatelist = output.split('\n')
		certificatelist.pop(-1)
		print('\n'.join(certificatelist))
		return True
	except Exception as e:
		print(e)
		return False

#获取可执行文件名
def getExecutableName(originpath):
	for dirpath, dirnames, filenames in os.walk(originpath):
		if dirpath[dirpath.rfind('.'):] == '.app':
			return dirpath[dirpath.rfind(os.sep)+1:dirpath.rfind('.')]

#判断文件是不是macho
def is_macho(file):
	if os.path.isfile(file) == False:
		return False

	magic = None
	with open(file, 'rb') as f:
		raw = f.read()
		magic = raw[:4]
	magic = list(magic)

	magics = [
            [0xFE, 0xED, 0xFA, 0xCE],
            [0xCE, 0xFA, 0xED, 0xFE],
            [0xFE, 0xED, 0xFA, 0xCF],
            [0xCF, 0xFA, 0xED, 0xFE],
            [0xCA, 0xFE, 0xBA, 0xBE],
            [0xBE, 0xBA, 0xFE, 0xCA],
            ]
	return any(m == magic for m in magics)

#文件是否需要签名
def isneedsign(filename, extrapath):
	full_path = os.path.join(extrapath,filename)
	if full_path.count('.appex'):#跳过appex
		return False
	# print("isneedsign filename:",full_path)#文件是否需要签名
	# (status, output) = subprocess.getstatusoutput('file %s' % (full_path))
	# if output.find("Mach-O")>=0:
	# 	# print(filename,"isneedsign")#macho文件肯定需要签名
	# 	return True
	if is_macho(full_path):
		# print(filename,"isneedsign")#macho文件肯定需要签名
		return True

	for signextension in signextensions:
		if signextension == filename[filename.rfind('.'):] or executableName == filename[filename.rfind(os.sep)+1:]:
			return True

	return False


# 生成appex的权限entitlements文件
def gen_plugins_entitlement(appex_path,appex_name,extrapath):

	(status, output) = subprocess.getstatusoutput(f'codesign -d --entitlements - {extrapath}/{appex_path} > {extrapath}/{appex_name}.plist' )
	if status == 0:
		print('gen_plugins_entitlement successed',  appex_path)
		return True
	else:
		print(output)
		return False

def codesign_plugins(extrapath):

	for appex_path,appex_name in appexs.items():
		entilement = os.path.join(extrapath,appex_name)+".plist"
		if not codesign(certificate,entilement,appex_path,extrapath):
			return False
	return True


#签名
def codesign(certificate,entilement,signObj,extrapath):
	# print("extrapath===",extrapath,",signObj=",signObj)
	# if signObj.endswith(os.sep):#目录也要签名?
	# 	return True

	(status, output) = subprocess.getstatusoutput('codesign -f -s "%s" --entitlements "%s" "%s"' % (certificate,entilement,os.path.join(extrapath,signObj)))
	if status == 0:
		print('replacing %s existing signature successed' % signObj)
		return True
	else:
		print(output)
		return False

#开始签名
def startsign(certificate,entilement,zfilelist,extrapath):
	print("----------------开始签名----------------")
	for filename in zfilelist:
		if isneedsign(filename, extrapath):
			if not codesign(certificate,entilement,filename,extrapath):
				print("codesign 0 fail")
				return False

	if not codesign_plugins(extrapath):
		print("codesign codesign_plugins fail")
		return False

	if not codesign(certificate,entilement,os.path.join('Payload',executableName+'.app'),extrapath):
		print("codesign 1 fail")
		return False
	return True

#zip压缩
def zipcompress(originpath,destinationzfile):
	resignedzfile = zipfile.ZipFile(destinationzfile,'w',zipfile.ZIP_DEFLATED)
	for dirpath, dirnames, filenames in os.walk(originpath):
		fpath = dirpath.replace(originpath,'')
		fpath = fpath and fpath + os.sep or ''
		for filename in filenames:
			resignedzfile.write(os.path.join(dirpath, filename), fpath+filename)
	resignedzfile.close()

#验证签名
def verifySignature(extralfilepath):
	for dirpath, dirnames, filenames in os.walk(extralfilepath):
		if dirpath[dirpath.rfind('.'):] == '.app':
			print("verifySignature:",dirpath)
			(status,output) = subprocess.getstatusoutput('codesign -v %s' % dirpath)
			if len(output) == 0:
				return True
			else:
				print(output)
				return False
	return False

#准备签名证书
def prepareCertificate():
	#获取证书列表
	if not getCertificates():
		return False

	try:
		certificateindexstr = input('请输入签名证书序号：').strip()
		certificateindex = int(certificateindexstr)
		if certificateindex < 1 or certificateindex > len(certificatelist):
			print('签名证书选择有误,请重试')
			return False
		else:
			selcert = certificatelist[certificateindex-1]
			global certificate
			certificate = selcert[selcert.find('"')+1:selcert.rfind('"')]
			print("你选择的签名证书是："+certificate)
	except Exception as e:
		print('签名证书选择有误,请重试')
		return False

#准备参数
def prepareArgsOptions():
	print(sys.argv)
	showhelp,showversion = False, False
	try:
		opts, args = getopt.getopt(sys.argv[1:],'hvi:a:o:c:p:e:b:')
	except Exception as e:
		print('参数不正确，请仔细检查！\n使用"resign -h"命令来查看帮助')
		sys.exit(0)
	for op, value in opts:
		if op == '-i':
			global zipFilePath
			zipFilePath = value
		elif op == '-a':
			global appPath
			appPath = value
		elif op == '-o':
			global outputPath
			outputPath = value
		elif op == '-c':
			global certificate
			certificate = value
		elif op == '-p':
			global mobileprovision
			mobileprovision = value
		elif op == '-e':
			global entilement
			entilement = value
		elif op == '-b':
			global newBundleIdentifier
			newBundleIdentifier = value
		elif op == '-h':
			showhelp = True
		elif op == '-v':
			showversion = True
	if len(sys.argv) == 1 or showhelp:
		print('''
Usage: resign -i <input.ipa>|-a <input.app> -c "<certificate-name>" -p <provision-file-path>
where options are:
  -o <output-file-path>       resigned file output path
  -e <entitlements-file-path> entitlements.plist path
  -b <new-bundle-identifier>  new bundle id match with mobileprovsion
  -h                          show help information
  -v                          show resign tool version
			''')
		sys.exit(0)
	elif showversion:
		print(version)
		sys.exit(0)
	else:
		if zipFilePath == None and appPath == None:
			zipFilePath = input('请拖拽ipa or app到此：').strip()

		if certificate == None:
			prepareCertificate()
		if mobileprovision == None:
			mobileprovision = input('请拖拽mobileprovsion到此：').strip()
		if zipFilePath and not os.path.isfile(zipFilePath):
			print('待签名ipa路径不正确，请仔细检查！ipa_path={}'.format(zipFilePath))
			sys.exit(0)
		# if appPath and not os.path.exists(appPath):
		# 	print('待签名app路径不正确，请仔细检查！appPath={}'.format(appPath))
		# 	sys.exit(0)

		# if not os.path.isfile(zipFilePath) and not os.path.ispath(appPath):
		# 	print('待签名ipa路径不正确，请仔细检查！')
		# 	sys.exit(0)
		if not os.path.isfile(mobileprovision):
			print('mobileprovsion路径不正确，请仔细检查！')
			sys.exit(0)


def getallfiles(path, is_fullname=False):
	allfile=[]
	for dirpath,dirnames,filenames in os.walk(path):
		for dir in dirnames:
			allfile.append(os.path.join(dirpath,dir))
		for name in filenames:
			allfile.append(os.path.join(dirpath, name))
	return allfile

def main():

	homedir = os.environ['HOME']
	extrapath = '%s/Payload_temp_%s/' % (homedir,str(time.time()))

	#准备参数
	prepareArgsOptions()

	global outputPath
	if outputPath == None:
		outputPath = zipFilePath[:zipFilePath.rfind('.')] + '_resigned.ipa'
	elif not '.ipa' in outputPath:
		if not os.path.exists(outputPath):
			print('输出文件路径有误，请仔细检查！')
			sys.exit(0)
			return
		outputPath = os.path.join(outputPath,zipFilePath[zipFilePath.rfind(os.sep)+1:zipFilePath.rfind('.')]+'_resigned.ipa')

	#兼容.app
	if zipFilePath == None and appPath != None:
		payloadPath = extrapath+"Payload"
		p,app_name=os.path.split(appPath)
		os.makedirs(payloadPath)
		shutil.copytree(appPath, payloadPath+os.sep+app_name)
		zfilelist = getallfiles(extrapath)

	else:
	#.ipa
		originzfile = zipfile.ZipFile(zipFilePath,'r')
		zfilelist = originzfile.namelist()
		zfilelist.reverse()

		#解压到临时目录
		originzfile.extractall(extrapath)
		# print(extrapath+"__MACOSX")
		if os.path.exists(extrapath + "__MACOSX"):
			shutil.rmtree(extrapath + "__MACOSX")#垃圾文件

	global appexs
	#移除垃圾文件名字
	to_del_list = []
	for i in range(0, len(zfilelist)):
		if zfilelist[i].find("__MACOSX")>=0:
			# print("remove1", zfilelist[i])
			# zfilelist.remove(l)
			to_del_list.append(zfilelist[i])
			# del zfilelist[i]
		elif zfilelist[i].endswith(".appex/"):
			appex = zfilelist[i].split(".appex")[0].split(os.sep)[-1]
			appexs[zfilelist[i]] = appex
	for l in to_del_list:
		zfilelist.remove(l)

	for appex_path,appex_name in appexs.items():
		if not gen_plugins_entitlement(appex_path, appex_name, extrapath):
			print(f"生成{appex_name} appex entitlements.plist文件失败!")
			shutil.rmtree(extrapath)
			return False
	#修改BundleIdentifier
	if newBundleIdentifier != None:
		modifyBundleIdentifer(extrapath,newBundleIdentifier)

	#拷贝mobileprovsion
	copyprovsion2appdir(extrapath, mobileprovision)

	global entilement
	if entilement == None:
		entilement = extrapath + "entitlements.plist"
		#生成entitlement.plist文件
		if not generateentitlements(mobileprovision,entilement):
			print("生成app entitlements.plist文件失败!")
			#关闭zipfile
			originzfile.close()
			#删除临时解压目录
			shutil.rmtree(extrapath)
			return False

	# entilement = "/Users/ne0/Downloads/11111_resigned/Payload/wx.xml"
	
	#获取可执行文件名
	global executableName
	executableName = getExecutableName(extrapath)
	if executableName == None or executableName == '':
		print("获取可执行文件名失败!")
		#关闭zipfile
		originzfile.close()
		#删除临时解压目录
		shutil.rmtree(extrapath)
		return False

	try:
		#开始签名
		if zfilelist != None and startsign(certificate,entilement,zfilelist,extrapath):
			print("-------------签名完成，开始验证签名-------------")
			if verifySignature(extrapath):
				print("-------------验签成功，开始打包-------------")
				zipcompress(extrapath,outputPath)
				print("🚀 重签名打包成功,请查看：%s" % outputPath)
			else:
				print("-----------------验签失败，请重试---------------")
		else:
			print("----------------签名失败，请重试----------------")
	finally:
		#关闭zipfile
		if zipFilePath:
			originzfile.close()
		#删除临时解压目录
		shutil.rmtree(extrapath)

if __name__ == '__main__':
	main()
```	

# 使用说明
```
xxxx/resign.py -i /Users/king/Desktop/dengpao17.ipa -c "iPhone Distribution: DONGFENG MOTOR FINANCE COMPANY LIMITED" -p /Users/king/Desktop/p12/DON/embedded.mobileprovision
```
#### `/Users/king/Desktop/dengpao17.ipa`要重签的ipa文件路径
#### `"iPhone Distribution: DONGFENG MOTOR FINANCE COMPANY LIMITED"`签名证书钥匙串查看证书详情获取
#### `/Users/king/Desktop/p12/DON/embedded.mobileprovision`embedded.mobileprovisionwen文件路径

#### 上机操作
```
~/SourceCode » python3 resign.py -i /Users/rby/Downloads/Misc/testResign/h3lix-RC6.ipa -c "iPhone Distribution: Shanghai Runba  Information Technology Co.,Ltd. (4T57E5CWND)" -p /Users/rby/Downloads/Misc/testResign/embedded.mobileprovision
['resign.py', '-i', '/Users/rby/Downloads/Misc/testResign/h3lix-RC6.ipa', '-c', 'iPhone Distribution: Shanghai Runba  Information Technology Co.,Ltd. (4T57E5CWND)', '-p', '/Users/rby/Downloads/Misc/testResign/embedded.mobileprovision']
----------------开始签名----------------
replacing Payload/h3lix.app/h3lix existing signature successed
replacing Payload/h3lix.app/tar existing signature successed
replacing Payload/h3lix.app/launchctl existing signature successed
replacing Payload/h3lix.app/ existing signature successed
replacing Payload/h3lix.app existing signature successed
-------------签名完成，开始验证签名-------------
verifySignature: /Users/rby/Payload_temp_1591777772.652998/Payload/h3lix.app
-------------验签成功，开始打包-------------
🚀 重签名打包成功,请查看：/Users/rby/Downloads/Misc/testResign/h3lix-RC6_resigned.ipa
----------------------------------------------------------------------------------------------------
~/SourceCode » 

```
#### 相关文件路径

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200610163307.png)

#### 如何获取embedded.mobileprovision文件，新建一个项目，然后配置好Bundle Id和证书，编译一下，然后打开app所在的目录，把app的后缀名改为.zip,再解压，就能看到embedded.mobileprovision这个文件了。
