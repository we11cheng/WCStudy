### Mac 安装和管理多个 Python 版本
#### 安装 Homebrew
####官网地址：brew.sh/ 获取安装指令，进行安装：
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
#### 复制代码Homebrew安装成功后，会自动创建目录 /usr/local/Cellar 来存放Homebrew安装的程序

#### 安装 pyenv

```
brew update
brew install pyenv
pyenv -v # 安装之后查看 pyenv 版本，确认是否安装成功
复制代码3、安装 & 管理多个 Python
pyenv install 2.7.15
pyenv install 3.7.3
pyenv versions # 所有已经安装的版本
```
#### 复制代码注意：在 MacOS 10.14 中，可能出现以下错误：
```
zipimport.ZipImportError: can't decompress data; zlib not available
make: *** [install] Error 1
```

#### 解决方案：
```
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
# 此时再安装试试
pyenv install 3.7.3
# 查看所有已经安装的版本，注：星号指定当前的版本
pyenv versions

```
### 查看所有已经安装的版本，注：星号指定当前的版本
#### PS：默认安装路径：~/.pyenv/shims/python
#### 常用的命令

```
使用方式: pyenv <命令> [<参数>]

命令:
  commands    查看所有命令
  local       设置或显示本地的 Python 版本（当前目录及其子目录）
  global      设置或显示全局 Python 版本
  shell       设置或显示 shell 指定的 Python 版本（本次会话）
  install     安装指定 Python 版本
  uninstall   卸载指定 Python 版本)
  version     显示当前的 Python 版本及其本地路径
  versions    查看所有已经安装的版本
  which       显示安装路径
```
#### 切换版本

```
pyenv global 3.7.3 # 不建议全局切换
python -V  # 验证一下是否切换成功
pyevn global system  # 切换回系统版本
pyenv local 3.7.3  # 当前目录及其目录切换
python -V  # 验证一下是否切换成功
pyenv local --unset  # 解除local设置
pyenv shell 3.7.3  # 当前shell会话切换
python -V  # 验证一下是否切换成功
pyenv shell --unset  # 解除shell设置
```
#### 切换不成功
如果遇到切换之后，Python版本还是系统的默认版本的话，就需要配置一下环境变量，在 ~/.zshrc 或 ~/.bash_profile 文件最后写入：

```
export PYENV_ROOT=~/.pyenv
export PATH=$PYENV_ROOT/shims:$PATH
if which pyenv > /dev/null;
  then eval "$(pyenv init -)";
fi
```
#### 使配置生效(zsh)
```
source ~/.zshrc
```
### 或者(非zsh)
```
source ~/.bash_profile
```
#### 题外话，安装virtualenv环境。

```
pip3 install virtualenv
```
#### 安装完成检测版本是否安装成功。

```
virtualenv --version
```
#### 第一步 cd 到你要创建虚拟环境的目录,创建虚拟环境目录文件夹。
```
virtualenv test_env01
```
#### 第二步激活虚拟环境。
```
source test_env01/bin/activate
```
#### 虚拟环境操作。
```
deactivate
```