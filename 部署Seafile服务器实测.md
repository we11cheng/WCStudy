### 部署Seafile服务器实测

```
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile# ls
installed  seafile-server-6.2.5
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile# cd seafile-server-6.2.5/
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# ls
check_init_admin.py  reset-admin.sh  runtime  seaf-fsck.sh  seaf-fuse.sh  seaf-gc.sh  seafile  seafile.sh  seahub
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# python --version
Python 2.7.12
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# ls
check_init_admin.py  runtime       seaf-fuse.sh  seafile     seahub     setup-seafile-mysql.py  setup-seafile.sh
reset-admin.sh       seaf-fsck.sh  seaf-gc.sh    seafile.sh  seahub.sh  setup-seafile-mysql.sh  upgrade
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# python-setuptools --version
python-setuptools: command not found
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# apt-get install python-setuptools
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  python-pkg-resources
Suggested packages:
  python-setuptools-doc
The following packages will be upgraded:
  python-pkg-resources python-setuptools
2 upgraded, 0 newly installed, 0 to remove and 106 not upgraded.
Need to get 464 kB of archives.
After this operation, 608 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial/main amd64 python-setuptools all 33.1.1-1+certbot~xenial+1 [297 kB]
Get:2 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial/main amd64 python-pkg-resources all 33.1.1-1+certbot~xenial+1 [166 kB]
Fetched 464 kB in 28s (16.1 kB/s)                                                                                            
(Reading database ... 115323 files and directories currently installed.)
Preparing to unpack .../python-setuptools_33.1.1-1+certbot~xenial+1_all.deb ...
Unpacking python-setuptools (33.1.1-1+certbot~xenial+1) over (20.7.0-1) ...
Preparing to unpack .../python-pkg-resources_33.1.1-1+certbot~xenial+1_all.deb ...
Unpacking python-pkg-resources (33.1.1-1+certbot~xenial+1) over (20.7.0-1) ...
Setting up python-pkg-resources (33.1.1-1+certbot~xenial+1) ...
Setting up python-setuptools (33.1.1-1+certbot~xenial+1) ...
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# apt-get install python-imaging
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  liblcms2-2 libwebp5 libwebpmux1 python-pil
Suggested packages:
  liblcms2-utils python-pil-doc python-pil-dbg
The following NEW packages will be installed:
  liblcms2-2 libwebp5 libwebpmux1 python-imaging python-pil
0 upgraded, 5 newly installed, 0 to remove and 106 not upgraded.
Need to get 633 kB of archives.
After this operation, 2,281 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/main amd64 liblcms2-2 amd64 2.6-3ubuntu2 [137 kB]
Get:2 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/main amd64 libwebp5 amd64 0.4.4-1 [165 kB]
Get:3 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/main amd64 libwebpmux1 amd64 0.4.4-1 [14.2 kB]
Get:4 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/main amd64 python-pil amd64 3.1.2-0ubuntu1.1 [312 kB]
Get:5 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/universe amd64 python-imaging all 3.1.2-0ubuntu1.1 [4,596 B]
Fetched 633 kB in 0s (9,727 kB/s)    
Selecting previously unselected package liblcms2-2:amd64.
(Reading database ... 115340 files and directories currently installed.)
Preparing to unpack .../liblcms2-2_2.6-3ubuntu2_amd64.deb ...
Unpacking liblcms2-2:amd64 (2.6-3ubuntu2) ...
Selecting previously unselected package libwebp5:amd64.
Preparing to unpack .../libwebp5_0.4.4-1_amd64.deb ...
Unpacking libwebp5:amd64 (0.4.4-1) ...
Selecting previously unselected package libwebpmux1:amd64.
Preparing to unpack .../libwebpmux1_0.4.4-1_amd64.deb ...
Unpacking libwebpmux1:amd64 (0.4.4-1) ...
Selecting previously unselected package python-pil:amd64.
Preparing to unpack .../python-pil_3.1.2-0ubuntu1.1_amd64.deb ...
Unpacking python-pil:amd64 (3.1.2-0ubuntu1.1) ...
Selecting previously unselected package python-imaging.
Preparing to unpack .../python-imaging_3.1.2-0ubuntu1.1_all.deb ...
Unpacking python-imaging (3.1.2-0ubuntu1.1) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
Setting up liblcms2-2:amd64 (2.6-3ubuntu2) ...
Setting up libwebp5:amd64 (0.4.4-1) ...
Setting up libwebpmux1:amd64 (0.4.4-1) ...
Setting up python-pil:amd64 (3.1.2-0ubuntu1.1) ...
Setting up python-imaging (3.1.2-0ubuntu1.1) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# apt-get install python-ldap
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Suggested packages:
  python-ldap-doc python-pyasn1
The following NEW packages will be installed:
  python-ldap
0 upgraded, 1 newly installed, 0 to remove and 106 not upgraded.
Need to get 67.8 kB of archives.
After this operation, 319 kB of additional disk space will be used.
Get:1 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/universe amd64 python-ldap amd64 2.4.22-0.1 [67.8 kB]
Fetched 67.8 kB in 0s (1,755 kB/s)
Selecting previously unselected package python-ldap.
(Reading database ... 115501 files and directories currently installed.)
Preparing to unpack .../python-ldap_2.4.22-0.1_amd64.deb ...
Unpacking python-ldap (2.4.22-0.1) ...
Setting up python-ldap (2.4.22-0.1) ...
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# apt-get install python-urllib3
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  python-asn1crypto python-cffi-backend python-cryptography python-enum34 python-idna python-ipaddress python-openssl
  python-six
Suggested packages:
  python-cryptography-doc python-cryptography-vectors python-enum34-doc python-openssl-doc python-openssl-dbg python-ntlm
  python-socks
The following NEW packages will be installed:
  python-asn1crypto python-cffi-backend python-cryptography python-enum34 python-idna python-ipaddress python-openssl
  python-six python-urllib3
0 upgraded, 9 newly installed, 0 to remove and 106 not upgraded.
Need to get 596 kB of archives.
After this operation, 3,444 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/main amd64 python-enum34 all 1.1.2-1 [35.8 kB]
Get:2 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial/main amd64 python-asn1crypto all 0.22.0-2+ubuntu16.04.1+certbot+1 [70.2 kB]
Get:3 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial/main amd64 python-cffi-backend amd64 1.10.0-0.1+ubuntu16.04.1+certbot+1 [71.5 kB]
Get:4 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial/main amd64 python-idna all 2.5-1+ubuntu16.04.1+certbot+1 [31.5 kB]
Get:5 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial/main amd64 python-ipaddress all 1.0.17-1+certbot~xenial+1 [18.3 kB]
Get:6 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial/main amd64 python-six all 1.11.0-1+ubuntu16.04.1+certbot+1 [11.9 kB]
Get:7 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial/main amd64 python-cryptography amd64 1.9-1+ubuntu16.04.1+certbot+2 [212 kB]
Get:8 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial/main amd64 python-openssl all 17.3.0-1~0+ubuntu16.04.1+certbot+1 [47.5 kB]
Get:9 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial/main amd64 python-urllib3 all 1.21.1-1+ubuntu16.04.1+certbot+1 [97.2 kB]
Fetched 596 kB in 34s (17.2 kB/s)                                                                                            
Selecting previously unselected package python-asn1crypto.
(Reading database ... 115543 files and directories currently installed.)
Preparing to unpack .../python-asn1crypto_0.22.0-2+ubuntu16.04.1+certbot+1_all.deb ...
Unpacking python-asn1crypto (0.22.0-2+ubuntu16.04.1+certbot+1) ...
Selecting previously unselected package python-cffi-backend.
Preparing to unpack .../python-cffi-backend_1.10.0-0.1+ubuntu16.04.1+certbot+1_amd64.deb ...
Unpacking python-cffi-backend (1.10.0-0.1+ubuntu16.04.1+certbot+1) ...
Selecting previously unselected package python-enum34.
Preparing to unpack .../python-enum34_1.1.2-1_all.deb ...
Unpacking python-enum34 (1.1.2-1) ...
Selecting previously unselected package python-idna.
Preparing to unpack .../python-idna_2.5-1+ubuntu16.04.1+certbot+1_all.deb ...
Unpacking python-idna (2.5-1+ubuntu16.04.1+certbot+1) ...
Selecting previously unselected package python-ipaddress.
Preparing to unpack .../python-ipaddress_1.0.17-1+certbot~xenial+1_all.deb ...
Unpacking python-ipaddress (1.0.17-1+certbot~xenial+1) ...
Selecting previously unselected package python-six.
Preparing to unpack .../python-six_1.11.0-1+ubuntu16.04.1+certbot+1_all.deb ...
Unpacking python-six (1.11.0-1+ubuntu16.04.1+certbot+1) ...
Selecting previously unselected package python-cryptography.
Preparing to unpack .../python-cryptography_1.9-1+ubuntu16.04.1+certbot+2_amd64.deb ...
Unpacking python-cryptography (1.9-1+ubuntu16.04.1+certbot+2) ...
Selecting previously unselected package python-openssl.
Preparing to unpack .../python-openssl_17.3.0-1~0+ubuntu16.04.1+certbot+1_all.deb ...
Unpacking python-openssl (17.3.0-1~0+ubuntu16.04.1+certbot+1) ...
Selecting previously unselected package python-urllib3.
Preparing to unpack .../python-urllib3_1.21.1-1+ubuntu16.04.1+certbot+1_all.deb ...
Unpacking python-urllib3 (1.21.1-1+ubuntu16.04.1+certbot+1) ...
Setting up python-asn1crypto (0.22.0-2+ubuntu16.04.1+certbot+1) ...
Setting up python-cffi-backend (1.10.0-0.1+ubuntu16.04.1+certbot+1) ...
Setting up python-enum34 (1.1.2-1) ...
Setting up python-idna (2.5-1+ubuntu16.04.1+certbot+1) ...
Setting up python-ipaddress (1.0.17-1+certbot~xenial+1) ...
Setting up python-six (1.11.0-1+ubuntu16.04.1+certbot+1) ...
Setting up python-cryptography (1.9-1+ubuntu16.04.1+certbot+2) ...
Setting up python-openssl (17.3.0-1~0+ubuntu16.04.1+certbot+1) ...
Setting up python-urllib3 (1.21.1-1+ubuntu16.04.1+certbot+1) ...
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# python3 --version
Python 3.5.2
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# apt-get install python-memcache
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Suggested packages:
  memcached
The following NEW packages will be installed:
  python-memcache
0 upgraded, 1 newly installed, 0 to remove and 106 not upgraded.
Need to get 17.1 kB of archives.
After this operation, 78.8 kB of additional disk space will be used.
Get:1 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/main amd64 python-memcache all 1.57-1 [17.1 kB]
Fetched 17.1 kB in 0s (570 kB/s)     
Selecting previously unselected package python-memcache.
(Reading database ... 115791 files and directories currently installed.)
Preparing to unpack .../python-memcache_1.57-1_all.deb ...
Unpacking python-memcache (1.57-1) ...
Setting up python-memcache (1.57-1) ...
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# ls
check_init_admin.py  runtime       seaf-fuse.sh  seafile     seahub     setup-seafile-mysql.py  setup-seafile.sh
reset-admin.sh       seaf-fsck.sh  seaf-gc.sh    seafile.sh  seahub.sh  setup-seafile-mysql.sh  upgrade
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# ./setup-seafile-mysql.sh
Checking python on this machine ...
  Checking python module: setuptools ... Done.
  Checking python module: python-imaging ... Done.
  Checking python module: python-mysqldb ... 
 python-mysqldb  is not installed, Please install it first.

On Debian/Ubuntu:

sudo apt-get install python-mysqldb

On CentOS/RHEL:

sudo yum install MySQL-python


Error occured during setup. 
Please fix possible problems and run the script again.

root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# apt-get install python-mysqldb
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Suggested packages:
  python-egenix-mxdatetime python-mysqldb-dbg
The following NEW packages will be installed:
  python-mysqldb
0 upgraded, 1 newly installed, 0 to remove and 106 not upgraded.
Need to get 42.4 kB of archives.
After this operation, 167 kB of additional disk space will be used.
Get:1 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/main amd64 python-mysqldb amd64 1.3.7-1build2 [42.4 kB]
Fetched 42.4 kB in 0s (833 kB/s)    
Selecting previously unselected package python-mysqldb.
(Reading database ... 115800 files and directories currently installed.)
Preparing to unpack .../python-mysqldb_1.3.7-1build2_amd64.deb ...
Unpacking python-mysqldb (1.3.7-1build2) ...
Setting up python-mysqldb (1.3.7-1build2) ...
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# ./setup-seafile-mysql.sh
Checking python on this machine ...
  Checking python module: setuptools ... Done.
  Checking python module: python-imaging ... Done.
  Checking python module: python-mysqldb ... Done.

-----------------------------------------------------------------
This script will guide you to setup your seafile server using MySQL.
Make sure you have read seafile server manual at

        https://github.com/haiwen/seafile/wiki

Press ENTER to continue
-----------------------------------------------------------------


What is the name of the server? It will be displayed on the client.
3 - 15 letters or digits
[ server name ] igwc

What is the ip or domain of the server?
For example: www.mycompany.com, 192.168.1.101
[ This server's ip or domain ] 47.98.177.125

Where do you want to put your seafile data?
Please use a volume with enough free space
[ default "/home/guanweicheng/wcseafile/seafile-data" ] 

Which port do you want to use for the seafile fileserver?
[ default "8082" ] 

-------------------------------------------------------
Please choose a way to initialize seafile databases:
-------------------------------------------------------

[1] Create new ccnet/seafile/seahub databases
[2] Use existing ccnet/seafile/seahub databases

[ 1 or 2 ] 1

What is the host of mysql server?
[ default "localhost" ] 

What is the port of mysql server?
[ default "3306" ] 

What is the password of the mysql root user?
[ root password ] 

verifying password of user root ...  done

Enter the name for mysql user of seafile. It would be created if not exists.
[ default "seafile" ] 

Enter the password for mysql user "seafile":
[ password for seafile ] 

Enter the database name for ccnet-server:
[ default "ccnet-db" ] 

Enter the database name for seafile-server:
[ default "seafile-db" ] 

Enter the database name for seahub:
[ default "seahub-db" ] 

---------------------------------
This is your configuration
---------------------------------

    server name:            igwc
    server ip/domain:       47.98.177.125

    seafile data dir:       /home/guanweicheng/wcseafile/seafile-data
    fileserver port:        8082

    database:               create new
    ccnet database:         ccnet-db
    seafile database:       seafile-db
    seahub database:        seahub-db
    database user:          seafile



---------------------------------
Press ENTER to continue, or Ctrl-C to abort
---------------------------------

Generating ccnet configuration ...

done
Successly create configuration dir /home/guanweicheng/wcseafile/ccnet.
Generating seafile configuration ...

Done.
done
Generating seahub configuration ...

----------------------------------------
Now creating seahub database tables ...

----------------------------------------

creating seafile-server-latest symbolic link ...  done




-----------------------------------------------------------------
Your seafile server configuration has been finished successfully.
-----------------------------------------------------------------

run seafile server:     ./seafile.sh { start | stop | restart }
run seahub  server:     ./seahub.sh  { start <port> | stop | restart <port> }

-----------------------------------------------------------------
If you are behind a firewall, remember to allow input/output of these tcp ports:
-----------------------------------------------------------------

port of seafile fileserver:   8082
port of seahub:               8000

When problems occur, Refer to

        https://github.com/haiwen/seafile/wiki

for information.


root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# ./seafile.sh start

[08/23/18 11:14:01] ../common/session.c(132): using config file /home/guanweicheng/wcseafile/conf/ccnet.conf
Starting seafile server, please wait ...
Seafile server started

Done.
root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# ./seahub.sh restart

Seahub is not running
LC_ALL is not set in ENV, set to en_US.UTF-8
Starting seahub at port 8000 ...

----------------------------------------
It's the first time you start the seafile server. Now let's create the admin account
----------------------------------------

What is the email for the admin account?
[ admin email ] 1396539283@qq.com

What is the password for the admin account?
[ admin password ] 

Enter the password again:
[ admin password again ] 



----------------------------------------
Successfully created seafile admin
----------------------------------------




Seahub is started

Done.

root@iZbp1ibd5qj8a3dwztxsf8Z:/home/guanweicheng/wcseafile/seafile-server-6.2.5# 
```
### 效果图
![](https://github.com/we11cheng/WCImageHost/raw/master/WX20180823-112617%402x.png)
### 参考链接 <https://manual-cn.seafile.com/deploy/using_mysql.html>