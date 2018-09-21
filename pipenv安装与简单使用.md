### pipenv安装与简单使用
### Pipenv 是 Python 项目的依赖管理器。其实它不是什么先进的理念和技术，如果你熟悉 Node.js 的 npm/yarn 或 Ruby 的 bundler，那么就非常好理解了，它在思路上与这些工具类似。尽管pip可以安装Python包，但仍推荐使用Pipenv，因为它是一种更高级的工具，可简化依赖关系管理的常见使用情况。

### 主要特性包含：

- 根据 Pipfile 自动寻找项目根目录。
- 如果不存在，可以自动生成 Pipfile 和 Pipfile.lock。
- 自动在项目目录的 .venv 目录创建虚拟环境。（当然这个目录地址通过设置WORKON_HOME改变）
- 自动管理 Pipfile 新安装和删除的包。
- 自动更新 pip。


### pipenv 都包含什么？
#### pipenv 是 Pipfile 主要倡导者、requests 作者 Kenneth Reitz 写的一个命令行工具，主要包含了Pipfile、pip、click、requests和virtualenv。Pipfile和pipenv本来都是Kenneth Reitz的个人项目，后来贡献给了pypa组织。Pipfile是社区拟定的依赖管理文件，用于替代过于简陋的 requirements.txt 文件。

### Pipfile的基本理念

### Pipfile 文件是 TOML 格式而不是 requirements.txt 这样的纯文本。一个项目对应一个 Pipfile，支持开发环境与正式环境区分。默认提供 default 和 development 区分。提供版本锁支持，存为 Pipfile.lock。click是Flask作者 Armin Ronacher 写的命令行库，现在Flask已经集成了它。

### 简单使用
#### pipenv兼容Python 2/3，我们这里以Mac下Python 3为例：

- 安装pipenv

```
brew install python3  # 如果已经安装了可以忽略
python3 -m pip install --upgrade --force-reinstall pip
pip3 install pipenv --user  # 推荐安装在个人目录下
export PATH="/Users/dongweiming/Library/Python/3.6/bin:$PATH"  # 把用户目录下bin放在最前面，这样可以直接使用pipenv了
```

- 使用pipenv

用一个空目录体验一下：

```
❯ mkdir test_pipenv
❯ cd test_pipenv
❯ pipenv install  # 创建一个虚拟环境
Creating a virtualenv for this project…
...
Installing setuptools, pip, wheel...done.

Virtualenv location: /Users/dongweiming/.virtualenvs/test_pipenv-GP_s2TW5
Creating a Pipfile for this project…
Pipfile.lock not found, creating…
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
Updated Pipfile.lock (c23e27)!
Installing dependencies from Pipfile.lock (c23e27)…
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 0/0 — 00:00:00
To activate this project's virtualenv, run the following:
 $ pipenv shell

❯ which python3
/usr/local/Cellar/python3/3.6.1/bin/python3  # 还是mac自带的Python

❯ pipenv shell  # 激活虚拟环境
Spawning environment shell (/bin/zsh). Use 'exit' to leave.
source /Users/dongweiming/.virtualenvs/test_pipenv-GP_s2TW5/bin/activate

❯ which python3  # 已经在虚拟环境里了
/Users/dongweiming/.virtualenvs/test_pipenv-GP_s2TW5/bin/python3

❯ exit  # 退出虚拟环境

❯ which python3
/usr/local/Cellar/python3/3.6.1/bin/python3
```

### 现在目录下Pipfile.lock已经更新了，包含了 elasticsearch-dsl、requests 和相关依赖的包信息。
