
# Pyenv安装

## 操作系统准备

> + 准备Linux最小系统即可。
> + 如果在虚拟机中克隆，MAC地址会变。 这里使用CentOS 6.5+

## pyenv安装方式

### git 安装

pyenv 的安装依赖于git 所以首先请确保本地安装了git

> 安装git

```bash
# yum install git -y
```

> 安装Python编译依赖

```bash
# yum -y install gcc make patch gdbm-devel openssl-devel sqlite-devel readline-devel zlib-devel bzip2-devel
```

> 创建用户python

```bash
# useradd python
```

> 使用python用户登录后安装Pyenv

```bash
pyenv-installer 是一个shell脚本。
$ curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash

默认安装到了当前用户的~/.pyenv 下面

[python@python ~]$ ls .pyenv/
bin           COMMANDS.md  CONDUCT.md  LICENSE   plugins  README.md  src                  test
CHANGELOG.md  completions  libexec     Makefile  pyenv.d  shims      terminal_output.png  versions
[python@python ~]$
```

> **注意：**

+ 在 <https://github.com/pyenv/pyenv-installer> 有安装文档
+ 如果curl出现 curl: (35) SSL connect error ，是nss版本低的问题，更新它。 可能需要配置一个有较新包的yum源

```bash
[updates]
name=CentOS-Updates
baseurl=https://mirrors.aliyun.com/centos/6.9/os/x86_64
gpgcheck=0
```

+ 然后更新nss # yum update nss

> 在python用户的~/.bash_profile中追加

```bash
export PATH="/home/python/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
$ source ~/.bash_profile
```

+ 这样当用户启动的时候，会执行用户的.bash_profile中的脚本，就会启动pyenv。 安装好的pyenv就在~/.pyenv中

### 离线安装

> 首先从github上克隆项目

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
git clone https://github.com/pyenv/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
git clone https://github.com/pyenv/pyenv-update.git ~/.pyenv/plugins/pyenv-update
git clone https://github.com/pyenv/pyenv-which-ext.git ~/.pyenv/plugins/pyenv-which-ext
```

+ 可以把克隆的目录打包，方便以后离线使用。

```bash
$ vim ~/.bash_profile
export PATH="/home/python/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
$ source ~/.bash_profile
```

# Pyenv的使用

## python 版本及path路径

```bash
python --version
python -V
echo $PATH
```

+ 可以看到当前系统Python路径

## pyenv 命令

```bash
[python@localhost ~]$ pyenv --help
Usage: pyenv <command> [<args>]

Some useful pyenv commands are:
   commands    List all available pyenv commands
   local       Set or show the local application-specific Python version
   global      Set or show the global Python version
   shell       Set or show the shell-specific Python version
   install     Install a Python version using python-build
   uninstall   Uninstall a specific Python version
   rehash      Rehash pyenv shims (run this after installing executables)
   version     Show the current Python version and its origin
   versions    List all Python versions available to pyenv
   which       Display the full path to an executable
   whence      List all Python versions that contain the given executable

See \`pyenv help <command>\' for information on a specific command.
For full documentation, see: https://github.com/pyenv/pyenv#readme
[python@localhost ~]$
```

列出所有可用版本

```bash
$ pyenv install --list
$
```

在线安装指定版本

从官网下载对应的版本压缩包到`/tmp/目录`，然后在`/tmp/目录`执行编译安装，安装到`~/.pyenv/versions/`下面

```bash
[python@python ~]$ pyenv install 3.5.3
[python@python ~]$ ls .pyenv/versions/
3.5.3
[python@python ~]$
[python@python ~]$ pyenv versions
* system (set by /home/python/.pyenv/version)
  3.5.3
[python@python ~]$
```

这样的安装可能较慢，为了提速，可是选用cache方法。

正常的安装过程是：

+ 从官网下载对应的版本压缩包到`/tmp/目录`
+ 然后在`/tmp/目录` 执行编译安装
+ 最终是安装到`~/.pyenv/versions/`下面

比如：

```bash
# 利用-v可以显示详细的安装过程
$ pyenv install 3.4.4 `[-v]`
Downloading Python-3.4.4.tgz...
-> https://www.python.org/ftp/python/3.4.4/Python-3.4.4.tgz
Installing Python-3.4.4...
Installed Python-3.4.4 to /root/.pyenv/versions/3.4.4
```

**cache**:

但是有个问题是有时候从官网下载tgz 包很慢，在不同的主机安装的时候重复操作是个很头疼的问题，这个时候 ./pyenv/cache 目录就能发挥它的作用

+ 提前从官网下载需要安装的包
+ copy到 ./pyenv/cache 中
+ 执行 pyenv install version-number -v

上面的version-number就是具体待安装的包的版本号， -v 展示安装过程

如果安装上面的操作执行就会省去下载的过程，直接执行 编译安装 ，会快很多。 其实上面的安装就是采用的这种

**cache 不生效**：

实际操作中发现，把下载的Python包放到了 cache 目录，但是还是去执行了下载过程

添加 -v 参数安装的时候看到下载的文件名字是 'Python-3.4.4.tgz'，如果把这个文件名copy到 ~/.pyenv/cache/ 下面的是不起作用的，还是会继续重新下载

查找问题后发现，-v 显示的是下载 'Python-3.4.4.tgz'， 但是在/tmp/python-xxxxxx.xxxx/ 目录下面却显示的是 'Python-3.4.4.tar.gz' 文件。

```bash
$ ls -l /tmp/python-build.20160608161435.16831
total 2960
-rw-r--r-- 1 root root 3031040 Jun  8 16:14 Python-3.4.4.tar.gz
```

所以把下载的 'Python-3.4.4.tgz' 改名为 'Python-3.4.4.tar.gz' 后放到~/.pyenv/cache/ 下面后，然后 pyenv install 3.4.4 -v 就不会重新下载了。

**注意**：不能采用把 Python-3.4.4.tgz 解压之后才压缩成 Python-3.4.4.tar.gz 的方式，因为这样的话会导致源文件的md5值发生变化。而校验失败重新下载

为了方便演示，请用客户端再打开两个会话窗口。 提前安装备用

```bash
$ pyenv install 3.6.4 -v
$
```

## pyenv的python版本控制

+ version 显示当前的python版本 versions 显示所有可用的python版本，和当前版本  

> global 全局设置

```bash
pyenv global 3.5.3
```

+ 可以看到所有受pyenv控制的窗口中都是3.5.3的python版本了。
+ 这里用global是作用于非root用户python用户上，如果是root用户安装，请不要使用global，否则影响太大。
+ 比如，这里使用的CentOS7.5就是Python2.7，使用了global就成了3.x，会带来很不好的影响。

```bash
pyenv global system
```

> shell 会话设置

```bash
pyenv shell 3.5.3
```

+ 影响只作用于当前会话

> local 本地设置

```bash
pyenv local 3.5.3
```

+ 使用pyenv local设置从当前工作目录开始向下递归都继承这个设置。

## Virtualenv 虚拟环境设置

+ 为什么要使用虚拟环境?
+ 因为刚才使用的Python环境都是一个公共的空间，如果多个项目使用不同Python版本开发，或者使用不同的Python版本部署运行，或者使用同样的版本开发的但不同项目使用了不同版本的库，等等这些问题都会带来冲突。最好的解决办法就是每一个项目独立运行自己的“独立小环境”中。

> 使用插件，在plugins/pyenv-virtualenv中

```bash
pyenv virtualenv 3.5.3 mag353
```

> 使用python3.5.3版本创建出一个独立的虚拟空间。

```bash
$ pyenv versions
* system (set by /home/python/.pyenv/version)
  3.5.3
  3.5.3/envs/mag353
  mag353
```

+ 可见在版本列表中存在，就和3.5.3是一样的，就是一个版本了。
+ 真实目录在~/.pyenv/versions/下，以后只要使用这个虚拟版本，包就会安装到这些对应的目录下去，而不是使用3.5.3。

```bash
[python@node ~]$ mkdir -p magedu/projects/web
[python@node ~]$ cd magedu/projects/web/
[python@node web]$ pyenv local mag353
(mag353) [python@node web]$ cd ..
[python@node projects]$ cd web/
(mag353) [python@node web]$
```

# pip 通用配置

+ pip 是Python的包管理工具，3.x的版本直接带了，可以直接使用。 和yum一样为了使用国内镜像，如下配置。

> Linux系统

```bash
$ mkdir ~/.pip
$ vim ~/.pip/pip.conf
[global]
index-url=https://mirrors.aliyun.com/pypi/simple/
trusted-host=mirrors.aliyun.com
```

> 在不同的虚拟环境中，安装redis包，使用pip list看看效果。

```bash
pip -V
pip install pkgname
```

+ windows系统 windows下pip的配置文件在~/pip/pip.ini，内容同上

# 安装ipython

> ipython 是增强的交互式Python命令行工具

```bash
pip install ipython
ipython
```

> Jupyter 是基于WEB的交互式笔记本，其中可以非常方便的使用Python。 安装Jupyter，也会安装ipython的

```bash
pip install jupyter
jupyter notebook help
jupyter notebook --ip=0.0.0.0 --no-browser
ss -tanl
```

# 导出包

> 虚拟环境的好处就在于和其他项目运行环境隔离。每一个独立的环境都可以使用pip命令导出已经安装的包，在另一个环境中安装这些包。

```bash
(mag353) [python@node web]$ pip freeze > requirement
(mag353) [python@node web]$ mkdir ~/magedu/projects/pro1
(mag353) [python@node web]$ cd ~/magedu/projects/pro1
[python@node pro1]$ pyenv install --list
[python@node pro1]$ pyenv install 3.6.4
[python@node pro1]$ pyenv virtualenv 3.6.4 mag364
[python@node pro1]$ pyenv local mag364
(mag364) [python@node pro1]$ mv ../web/requirement ./
(mag364) [python@node pro1]$ pip install -r requirement
```
