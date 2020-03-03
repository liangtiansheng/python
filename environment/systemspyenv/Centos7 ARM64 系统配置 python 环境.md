# Centos7 ARM64 系统配置 Python

在配置 Python 环境之前，首先列举一下底层环境，包括硬件和操作系统

+ 服务器是飞腾 2000+，CPU 是 ARM64 架构
+ 操作系统是公版 ARM64 Centos 操作系统

## 通过 YUM 源安装 Python

### 配置 Centos7 ARM64 源

```bash
[root@yidam ~]# cat /etc/yum.repos.d/CentOS-Base.repo|grep -Ev "^#|^$"
[base]
name=CentOS-$releasever - Base
baseurl=https://mirrors.aliyun.com/centos-altarch/$releasever/os/$basearch/
gpgcheck=1
gpgkey=http://archive.kernel.org/centos-vault/altarch/7.6.1810/os/aarch64/RPM-GPG-KEY-CentOS-7-aarch64
[updates]
name=CentOS-$releasever - Updates
baseurl=https://mirrors.aliyun.com/centos-altarch/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=http://archive.kernel.org/centos-vault/altarch/7.6.1810/os/aarch64/RPM-GPG-KEY-CentOS-7-aarch64
[extras]
name=CentOS-$releasever - Extras
baseurl=https://mirrors.aliyun.com/centos-altarch/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=http://archive.kernel.org/centos-vault/altarch/7.6.1810/os/aarch64/RPM-GPG-KEY-CentOS-7-aarch64
enabled=1
[centosplus]
name=CentOS-$releasever - Plus
baseurl=https://mirrors.aliyun.com/centos-altarch/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://archive.kernel.org/centos-vault/altarch/7.6.1810/os/aarch64/RPM-GPG-KEY-CentOS-7-aarch64
[root@yidam ~]#
```

### Python2.7 安装

安装

```bash
[root@yidam ~]# yum install python2 -y
```

测试

```bash
[root@yidam ~]# python2.7 -V
Python 2.7.5
[root@yidam ~]# python2.7 
Python 2.7.5 (default, Oct 31 2018, 18:48:32) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-36)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> print("hello world")
hello world
>>> 
```

卸载

```bash
[root@yidam ~]# yum remove python2 -y
```

### Python3.6 安装

安装

```bash
[root@yidam ~]# yum install python3 -y
```

测试

```bash
[root@yidam ~]# python3.6 -V
Python 3.6.8
[root@yidam ~]# python3.6
Python 3.6.8 (default, Aug  7 2019, 17:19:33) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-39)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> print("hello world")
hello world
>>> 
```

卸载

```bash
[root@yidam ~]# yum remove python3 -y
```

## 通过 Python 源码编译安装

### Python2.7 安装

源码下载

```bash
[root@yidam ~]# wget https://www.python.org/ftp/python/2.7.16/Python-2.7.16.tar.xz
[root@yidam ~]# ls
Python-2.7.16.tar.xz
[root@yidam ~]#
```

源码解压到指定的目录

```bash
[root@yidam ~]# mkdir /opt/python27
[root@yidam ~]# tar xf Python-2.7.16.tar.xz -C /opt/python27
[root@yidam ~]# ls /opt/python27/Python-2.7.16/
aclocal.m4    config.sub  configure.ac  Doc      Include     Lib      Mac              Misc     Objects  PC       pyconfig.h.in  README  setup.py
config.guess  configure   Demo          Grammar  install-sh  LICENSE  Makefile.pre.in  Modules  Parser   PCbuild  Python         RISCOS  Tools
[root@yidam ~]# 
```

解决 Python2.7 编译依赖

```bash
[root@yidam ~]# yum install gcc zlib zlib-devel openssl openssl-devel -y
```

Python2.7 编译

```bash
[root@yidam ~]# cd /opt/python27/Python-2.7.16/
[root@yidam Python-2.7.16]# mkdir /usr/local/python27
[root@yidam Python-2.7.16]# ./configure --enable-optimizations --prefix=/usr/local/python27
[root@yidam Python-2.7.16]# make && make install
```

测试

```bash
[root@yidam Python-2.7.16]# cd /usr/local/python27/
[root@yidam python27]# ls
bin  include  lib  share
[root@yidam python27]# ls bin/
2to3  idle  pydoc  python  python2  python2.7  python2.7-config  python2-config  python-config  smtpd.py
[root@yidam python27]# ./bin/python2.7 -V
Python 2.7.16
[root@yidam python27]# ./bin/python2.7
Python 2.7.16 (default, Feb 21 2020, 16:05:33) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-39)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> print("hello world")
hello world
>>> 
```

卸载

```bash
[root@yidam ~]# rm -rf /usr/local/python27/
```

### Python3.6 安装

源码下载

```bash
[root@yidam ~]# wget https://www.python.org/ftp/python/3.6.8/Python-3.6.8.tar.xz
[root@yidam ~]# ls
Python-3.6.8.tar.xz
[root@yidam ~]#
```

源码解压到指定的目录

```bash
[root@yidam ~]# mkdir /opt/python36
[root@yidam ~]# tar -xf Python-3.6.8.tar.xz -C /opt/python36
[root@yidam ~]# cd /opt/python36
[root@yidam python36]# ls Python-3.6.8/
aclocal.m4    config.sub  configure.ac  Grammar  install-sh  LICENSE  Makefile.pre.in  Modules  Parser  PCbuild   pyconfig.h.in  README.rst  Tools
config.guess  configure   Doc           Include  Lib         Mac      Misc             Objects  PC      Programs  Python         setup.py
[root@yidam python36]#
```

解决 Python3.6 编译依赖

```bash
[root@yidam ~]# yum install gcc zlib zlib-devel openssl openssl-devel -y
```

Python3.6 编译

```bash
[root@yidam ~]# cd /opt/python36/Python-3.6.8/
[root@yidam Python-3.6.8]# ./configure --enable-optimizations --prefix=/usr/local/python36
[root@yidam Python-3.6.8]# make && make install
```

测试

```bash
[root@yidam Python-3.6.8]# ls /usr/local/python36/bin/
2to3      easy_install-3.6  idle3.6  pip3.6  pydoc3.6  python3.6         python3.6m         python3-config  pyvenv-3.6
2to3-3.6  idle3             pip3     pydoc3  python3   python3.6-config  python3.6m-config  pyvenv
[root@yidam ~]# cd /usr/local/python36/
[root@yidam python36]# ./bin/python3.6 -V
Python 3.6.8
[root@yidam python36]# ./bin/python3.6
Python 3.6.8 (default, Feb 21 2020, 18:12:04) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-39)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> print("hello world")
hello world
>>>
```

卸载

```bash
[root@yidam ~]# rm -rf /usr/local/python36/
```

## 通过 Docker 容器运行 Python 环境

在配置 Docker Python 之前需要说明一下，容器应用很广泛，功能异常强大，下面只能将环境搭建起来，涉及到的容器基础知识需要读者自行学习。

```bash
[root@yidam ~]# yum install docker -y
[root@yidam ~]# systemctl start docker
```

配置 Docker 国内加速源

```bash
[root@yidam ~]# cat /etc/docker/daemon.json
{
  "registry-mirrors": ["https://dqtef2wb.mirror.aliyuncs.com"]
}
[root@yidam ~]#
```

+ 国内加速的网站有很多，而且多数网站都有自己的加速脚本，科学上网搜索即可

### 配置 Python2.7

拉取 python:2.7 镜像

```bash
[root@yidam ~]# docker pull python:2.7
Trying to pull repository docker.io/library/python ... 
2.7: Pulling from docker.io/library/python
Digest: sha256:b104660eb48e8b71148f670a9404ede4211cdc16ef33fad3e01be84d4e4817ec
Status: Downloaded newer image for docker.io/python:2.7
[root@yidam ~]#
```

查看 python:2.7 镜像是否拉取成功

```bash
[root@yidam ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python              2.7                 805f1ed43956        2 weeks ago         842 MB
python              3.5                 090e4d6705ae        2 weeks ago         853 MB
python              3.6                 4567cdde50f8        2 weeks ago         856 MB
[root@yidam ~]#
```

运行 python:2.7 容器

```bash
[root@yidam ~]# docker run -d -it python:2.7
75b624b9d9405b796a894b12e0dea302ab724fd91c677142415038f52fd53e6a
[root@yidam ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
75b624b9d940        python:2.7          "python2"           2 seconds ago       Up 1 second                             silly_northcutt
[root@yidam ~]#
```

查看 python:2.7 容器是否运行成功

```bash
[root@yidam ~]# docker exec -it silly_northcutt python -V
Python 2.7.17
[root@yidam ~]# 
```

进入 python:2.7 容器测试

```bash
[root@yidam ~]# docker run -d -it -v /tmp:/tmp python:2.7
6e2d75600e035f0105d311867ed5771af614f70b8926fd6f18bda09f8033b237
[root@yidam ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
6e2d75600e03        python:2.7          "python2"           6 seconds ago       Up 5 seconds                            laughing_jennings
[root@yidam ~]# docker exec -it laughing_jennings bash
root@6e2d75600e03:/# python /tmp/99.py 
1 * 1 =  1
1 * 2 =  2 2 * 2 =  4
1 * 3 =  3 2 * 3 =  6 3 * 3 =  9
1 * 4 =  4 2 * 4 =  8 3 * 4 = 12 4 * 4 = 16
1 * 5 =  5 2 * 5 = 10 3 * 5 = 15 4 * 5 = 20 5 * 5 = 25
1 * 6 =  6 2 * 6 = 12 3 * 6 = 18 4 * 6 = 24 5 * 6 = 30 6 * 6 = 36
1 * 7 =  7 2 * 7 = 14 3 * 7 = 21 4 * 7 = 28 5 * 7 = 35 6 * 7 = 42 7 * 7 = 49
1 * 8 =  8 2 * 8 = 16 3 * 8 = 24 4 * 8 = 32 5 * 8 = 40 6 * 8 = 48 7 * 8 = 56 8 * 8 = 64
1 * 9 =  9 2 * 9 = 18 3 * 9 = 27 4 * 9 = 36 5 * 9 = 45 6 * 9 = 54 7 * 9 = 63 8 * 9 = 72 9 * 9 = 81
root@6e2d75600e03:/#
```

删除容器 python:2.7

```bash
[root@yidam ~]# docker rm -f laughing_jennings
laughing_jennings
[root@yidam ~]# 
```

### 配置 Python3.5

拉取 python:3.5 镜像

```bash
[root@yidam ~]# docker pull python:3.5
Trying to pull repository docker.io/library/python ... 
3.5: Pulling from docker.io/library/python
Digest: sha256:24f39a9542911c29dc84b3c931b08735fb79562a8fbbc97339e41e4d5bb126f0
Status: Downloaded newer image for docker.io/python:3.5
[root@yidam ~]#
```

查看 python:3.5 镜像是否拉取成功

```bash
[root@yidam ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python              2.7                 805f1ed43956        2 weeks ago         842 MB
python              3.5                 090e4d6705ae        2 weeks ago         853 MB
docker.io/python    3.5                 090e4d6705ae        2 weeks ago         853 MB
python              3.6                 4567cdde50f8        2 weeks ago         856 MB
[root@yidam ~]#
```

运行 python:3.5 容器

```bash
[root@yidam ~]# docker run -d -it python:3.5
48da7ee333b09308db6f1a66747a0f7b2cb8541f3517b83083428f11fdebe520
[root@yidam ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
48da7ee333b0        python:3.5          "python3"           2 seconds ago       Up 1 second                             hopeful_hugle
[root@yidam ~]#
```

查看 python:3.5 容器是否运行成功

```bash
[root@yidam ~]# docker exec -it hopeful_hugle python -V
Python 3.5.9
[root@yidam ~]#
```

进入 python:3.5 容器测试

```bash
[root@yidam ~]# docker run -it -d -v /tmp/:/tmp/ python:3.5
860a3a217395504d1f2c246ec2471010da2e5b0e5081eb419cd9e2c9d893b75a
[root@yidam ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
860a3a217395        python:3.5          "python3"           2 seconds ago       Up 1 second                             condescending_nightingale
[root@yidam ~]# docker exec -it condescending_nightingale bash
root@860a3a217395:/# python3 /tmp/99.py 
1 * 1 =  1 
1 * 2 =  2 2 * 2 =  4 
1 * 3 =  3 2 * 3 =  6 3 * 3 =  9 
1 * 4 =  4 2 * 4 =  8 3 * 4 = 12 4 * 4 = 16 
1 * 5 =  5 2 * 5 = 10 3 * 5 = 15 4 * 5 = 20 5 * 5 = 25 
1 * 6 =  6 2 * 6 = 12 3 * 6 = 18 4 * 6 = 24 5 * 6 = 30 6 * 6 = 36 
1 * 7 =  7 2 * 7 = 14 3 * 7 = 21 4 * 7 = 28 5 * 7 = 35 6 * 7 = 42 7 * 7 = 49 
1 * 8 =  8 2 * 8 = 16 3 * 8 = 24 4 * 8 = 32 5 * 8 = 40 6 * 8 = 48 7 * 8 = 56 8 * 8 = 64 
1 * 9 =  9 2 * 9 = 18 3 * 9 = 27 4 * 9 = 36 5 * 9 = 45 6 * 9 = 54 7 * 9 = 63 8 * 9 = 72 9 * 9 = 81 
root@860a3a217395:/#
```

删除容器 python:3.5

```bash
[root@yidam ~]# docker rm -f condescending_nightingale
condescending_nightingale
[root@yidam ~]#
```

## Pyenv 控制 Python 版本

Pyenv 作为 Python 的多环境控制工具非常实用，大致有以下几个优点：

+ 允许您在每个用户的基础上更改全局 Python 版本
+ 提供对每个项目的 Python 版本的支持
+ 允许您使用环境变量覆盖 Python 版本
+ 一次搜索多个 Python 版本的命令

### Pyenv 安装

Pyenv 的安装依赖于 git 工具，所以首先确保本地安装了 git

```bash
[root@yidam ~]# yum install git -y
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Package git-1.8.3.1-21.el7_7.aarch64 already installed and latest version
Nothing to do
[root@yidam ~]#
```

安装 Python 编译依赖

```bash
[root@yidam ~]# yum install gcc zlib zlib-devel openssl openssl-devel -y
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Package gcc-4.8.5-39.el7.aarch64 already installed and latest version
Package zlib-1.2.7-18.el7.aarch64 already installed and latest version
Package zlib-devel-1.2.7-18.el7.aarch64 already installed and latest version
Package 1:openssl-1.0.2k-19.el7.aarch64 already installed and latest version
Package 1:openssl-devel-1.0.2k-19.el7.aarch64 already installed and latest version
Nothing to do
[root@yidam ~]#
```

创建用户 python

```bash
[root@yidam ~]# useradd python
[root@yidam ~]#
```

使用 python 用户登录后安装 Pyenv

```bash
[root@yidam ~]# su - python
[python@yidam ~]$ curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   148  100   148    0     0    152      0 --:--:-- --:--:-- --:--:--   152
100  2454  100  2454    0     0   1497      0  0:00:01  0:00:01 --:--:--  4382
Cloning into '/home/python/.pyenv'...
......
Cloning into '/home/python/.pyenv/plugins/pyenv-doctor'...
......
Cloning into '/home/python/.pyenv/plugins/pyenv-installer'...
......
Cloning into '/home/python/.pyenv/plugins/pyenv-update'...
......
Cloning into '/home/python/.pyenv/plugins/pyenv-virtualenv'...
......
Cloning into '/home/python/.pyenv/plugins/pyenv-which-ext'...
......
WARNING: seems you still have not added 'pyenv' to the load path.
# Load pyenv automatically by adding
# the following to ~/.bashrc:
export PATH="/home/python/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
[python@yidam ~]$ 
```

默认安装到了当前用户的 ~/.pyenv 下面

```bash
[python@yidam ~]$ ls .pyenv/
bin  CHANGELOG.md  COMMANDS.md  completions  CONDUCT.md  libexec  LICENSE  Makefile  plugins  pyenv.d  README.md  src  terminal_output.png  test
[python@yidam ~]$ 
```

在 python 用户的 ~/.bash_profile 中追加

```bash
[python@yidam ~]$ cat .bash_profile | tail -3
export PATH="/home/python/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
[python@yidam ~]$ 
[python@yidam ~]$ source .bash_profile 
[python@yidam ~]$
```

- 这样当该用户启动的时候，会执行用户的 .bash_profile 中的脚本，就会启动 pyenv。安装好的 pyenv 就在 ~/.pyenv 中

### Pyenv 的使用

#### Python 命令

查看命令帮助

```bash
[python@yidam ~]$ pyenv --help
Usage: pyenv <command> [<args>]

Some useful pyenv commands are:
   activate    Activate virtual environment
   commands    List all available pyenv commands
   deactivate   Deactivate virtual environment
   doctor      Verify pyenv installation and development tools to build pythons.
   exec        Run an executable with the selected Python version
   global      Set or show the global Python version
......
```

列出所有可用版本

```bash
[python@yidam ~]$ pyenv install --list
Available versions:
  2.1.3
  2.2.3
  2.3.7
......
  3.0.1
  3.1.0
  3.1.1
  3.1.2
......
```

#### 安装 Python

在线指定 Python 版本安装

_从官网下载对应的版本压缩包到 /tmp/ 目录，然后在 /tmp/ 目录执行编译安装，安装到 ~/.pyenv/versions/ 下面_

```bash
[python@yidam ~]$ pyenv install 3.5.3
Downloading Python-3.5.3.tar.xz...
-> https://www.python.org/ftp/python/3.5.3/Python-3.5.3.tar.xz
Installing Python-3.5.3...
WARNING: The Python bz2 extension was not compiled. Missing the bzip2 lib?
WARNING: The Python readline extension was not compiled. Missing the GNU readline lib?
WARNING: The Python sqlite3 extension was not compiled. Missing the SQLite3 lib?
Installed Python-3.5.3 to /home/python/.pyenv/versions/3.5.3

[python@yidam ~]$
```

查看安装好的 Python

```bash
[python@yidam ~]$ ls .pyenv/versions/
3.5.3
[python@yidam ~]$ pyenv versions
* system (set by /home/python/.pyenv/version)
  3.5.3
[python@yidam ~]$ 
```

#### Pyenv 的 Python 版本控制

Pyenv version 显示当前的 Python 版本，Pyenv versions 显示所有可用的 Python 版本和当前版本

```bash
[python@yidam ~]$ pyenv version
system (set by /home/python/.pyenv/version)
[python@yidam ~]$ 
[python@yidam ~]$ pyenv versions 
* system (set by /home/python/.pyenv/version)
  3.5.3
[python@yidam ~]$
```

global 全局设置

```bash
[python@yidam ~]$ pyenv global 3.5.3
[python@yidam ~]$ 
[python@yidam ~]$ pyenv version
3.5.3 (set by /home/python/.pyenv/version)
[python@yidam ~]$ 
[python@yidam ~]$ pyenv versions 
  system
* 3.5.3 (set by /home/python/.pyenv/version)
[python@yidam ~]$ 
```

+ 可以看到所有受 pyenv 控制的窗口中都是 3.5.3 的 python 版本了。
+ 这里用 global 是作用于非 root 用户 python 用户上，如果是 root 用户安装，请不要使用 global，否则影响太大。

shell 会话设置

```bash
[python@yidam ~]$ pyenv global system 
[python@yidam ~]$ pyenv versions
* system (set by /home/python/.pyenv/version)
  3.5.3
[python@yidam ~]$ 
[python@yidam ~]$ pyenv shell 3.5.3
[python@yidam ~]$ 
[python@yidam ~]$ pyenv versions
  system
* 3.5.3 (set by PYENV_VERSION environment variable)
[python@yidam ~]$
```

+ 影响只作用于当前会话

local 本地设置

```bash
[python@yidam ~]$ pyenv local 3.5.3
[python@yidam ~]$
```

+ 使用 pyenv local 设置从当前工作目录开始向下递归都继承这个设置

#### Virtualenv 虚拟环境设置

> 为什么要使用虚拟环境？
>
> 因为刚才使用的 Python 环境都是一个公共的空间，如果多个项目使用不同 Python 版本开发，或者使用不同的 Python 版本部署运行，或者使用同样的版本开发的但不同项目使用了不同版本的库，等等这些问题都会带来冲突。最好的解决办法就是每一个项目独立运行自己的独立小环境中。

使用插件，在 plugins/pyenv-virtualenv 中

```bash
[python@yidam ~]$ pyenv virtualenv 3.5.3 cec353
Requirement already satisfied: setuptools in /home/python/.pyenv/versions/3.5.3/envs/cec353/lib/python3.5/site-packages
Requirement already satisfied: pip in /home/python/.pyenv/versions/3.5.3/envs/cec353/lib/python3.5/site-packages
[python@yidam ~]$ 
```

使用 Python3.5.3 版本创建出了一个独立的虚拟空间

```bash
[python@yidam ~]$ pyenv versions
  system
* 3.5.3 (set by PYENV_VERSION environment variable)
  3.5.3/envs/cec353
  cec353
[python@yidam ~]$
```

+ 可见在版本列表中存在，就和 3.5.3 是一样的，就是一个版本了
+ 真实目录在 ~/.pyenv/versions/ 下，以后只要使用这个虚拟版本，包就会安装到这些对应的目录下面，而不是使用 3.5.3

```bash
[python@yidam ~]$ mkdir -p cec/greatwall/PK
[python@yidam ~]$ ls cec/greatwall/
PK
[python@yidam ~]$ cd cec/greatwall/PK/
[python@yidam PK]$ pwd
/home/python/cec/greatwall/PK
[python@yidam PK]$ 
[python@yidam PK]$ pyenv local cec353 
(cec353) [python@yidam PK]$ cd ..
[python@yidam greatwall]$ cd PK/
(cec353) [python@yidam PK]$
```

## Python 包管理工具 Pip

Pip 是 Python 的包管理工具，3.x 的版本直接带了，可以直接使用。

安装 Pip2

```bash
[root@yidam ~]# yum install python2-pip -y
```

安装 Pip3

```bash
[root@yidam ~]# yum install python3-pip.noarch -y
```

配置国内的 Pip 源

```bash
[root@yidam ~]# mkdir ~/.pip
[root@yidam ~]# vim .pip/pip.conf
[root@yidam ~]# cat .pip/pip.conf 
[global]
index-url=https://mirrors.aliyun.com/pypi/simple/
trusted-host=mirrors.aliyun.com
[root@yidam ~]# 
```

安装 redis 测试

```bash
[root@yidam ~]# pip3 install redis
WARNING: Running pip install with root privileges is generally not a good idea. Try `pip3 install --user` instead.
Collecting redis
  Downloading https://mirrors.aliyun.com/pypi/packages/f0/05/1fc7feedc19c123e7a95cfc9e7892eb6cdd2e5df4e9e8af6384349c1cc3d/redis-3.4.1-py2.py3-none-any.whl (71kB)
    100% |████████████████████████████████| 71kB 13.0MB/s 
Installing collected packages: redis
Successfully installed redis-3.4.1
[root@yidam ~]# 
```

安装 ipython 测试

```bash
[root@yidam ~]# pip3 install ipython
[root@yidam ~]# ipython
Python 3.6.8 (default, Aug  7 2019, 17:19:33) 
Type 'copyright', 'credits' or 'license' for more information
IPython 7.12.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: print("hello world")                                                                                                                                 
hello world

In [2]:  
```


