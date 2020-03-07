# Kylin ARM64 系统配置 Python

在配置 Python 环境之前，首先列举一下底层环境，包括硬件和操作系统

+ 服务器是飞腾 2000+，CPU 是 ARM64 架构
+ 操作系统是 ARM64 公版 Ubuntu，使用方式跟常用的 X86 架构下的 Ubuntu 类似

## 通过 APT 源安装 Python

### 配置 Ubuntu ARM64 源

```bash
ly@ubuntu:~$ sudo cat /etc/apt/sources.list|grep -v "^#"|grep -v "^$"
deb http://mirrors.aliyun.com/ubuntu-ports bionic main restricted
deb http://mirrors.aliyun.com/ubuntu-ports bionic-updates main restricted
deb http://mirrors.aliyun.com/ubuntu-ports bionic universe
deb http://mirrors.aliyun.com/ubuntu-ports bionic-updates universe
deb http://mirrors.aliyun.com/ubuntu-ports bionic multiverse
deb http://mirrors.aliyun.com/ubuntu-ports bionic-updates multiverse
deb http://mirrors.aliyun.com/ubuntu-ports bionic-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu-ports bionic-security main restricted
deb http://mirrors.aliyun.com/ubuntu-ports bionic-security universe
deb http://mirrors.aliyun.com/ubuntu-ports bionic-security multiverse
ly@ubuntu:~$
```

### Python2.7 安装

安装

```bash
ly@ubuntu:~$ sudo aptitude install python2.7 -y
```

测试

```bash
ly@ubuntu:~$ sudo python2.7
Python 2.7.17 (default, Nov  7 2019, 10:07:09) 
[GCC 7.4.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> print "hello world"
hello world
>>> 
```

卸载

```bash
ly@ubuntu:~$ sudo aptitude purge python2.7 -y
```

### Python3.6 安装

安装

```bash
ly@ubuntu:~$ sudo aptitude install python3.6 -y
```

测试

```bash
ly@ubuntu:~$ python3
Python 3.6.7 (default, Oct 22 2018, 11:32:17) 
[GCC 8.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> print("hello world")
hello world
>>> 
```

卸载

```bash
ly@ubuntu:~$ sudo aptitude purge python3 -y
```

+ 注意：这是卸载 python3.6 的方式，但是操作系统很多软件包都依赖 python3.6，谨慎操作

## 通过 Python 源码编译安装

### Python2.7 安装

源码下载

```bash
ly@ubuntu:~$ sudo wget https://www.python.org/ftp/python/2.7.16/Python-2.7.16.tar.xz
ly@ubuntu:~$ ls Python-2.7.16.tar.xz 
Python-2.7.16.tar.xz
ly@ubuntu:~$
```

源码解压到指定的目录

```bash
ly@ubuntu:~$ sudo mkdir /opt/python27
ly@ubuntu:~$ sudo tar xf Python-2.7.16.tar.xz -C /opt/python27
ly@ubuntu:~$ cd /opt/python27/Python-2.7.16/
ly@ubuntu:/opt/python27/Python-2.7.16$ ls
aclocal.m4    config.sub  configure.ac  Doc      Include     Lib      Mac              Misc     Objects  PC       pyconfig.h.in  README  setup.py
config.guess  configure   Demo          Grammar  install-sh  LICENSE  Makefile.pre.in  Modules  Parser   PCbuild  Python         RISCOS  Tools
ly@ubuntu:/opt/python27/Python-2.7.16$
```

解决 Python2.7 编译依赖

```bash
ly@ubuntu:~$ sudo aptitude install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget -y
```

Python2.7 编译

```bash
ly@ubuntu:~$ cd /opt/python27/Python-2.7.16/
ly@ubuntu:/opt/python27/Python-2.7.16$ ./configure --enable-optimizations --prefix=/usr/local/python27
ly@ubuntu:/opt/python27/Python-2.7.16$ sudo make && sudo make install
```

测试

```bash
ly@ubuntu:~$ cd /usr/local/python27/
ly@ubuntu:/usr/local/python27$ sudo ./bin/python2 -V
Python 2.7.16
ly@ubuntu:/usr/local/python27$ 
```

卸载

```bash
ly@ubuntu:~$ sudo rm -rf /usr/local/python27/
```

### Python3.5 安装

源码下载

```bash
ly@ubuntu:~$ sudo wget https://www.python.org/ftp/python/3.5.3/Python-3.5.3.tar.xz
```

源码解压到指定的目录

```bash
ly@ubuntu:~$ sudo mkdir /opt/python35
ly@ubuntu:~$ sudo tar xf Python-3.5.3.tar.xz -C /opt/python35
ly@ubuntu:~$ cd /opt/python35/Python-3.5.3/
ly@ubuntu:/opt/python35/Python-3.5.3$ ls
aclocal.m4    config.sub  configure.ac  Grammar  install-sh  LICENSE  Makefile.pre.in  Modules  Parser  PCbuild   pyconfig.h.in  README    Tools
config.guess  configure   Doc           Include  Lib         Mac      Misc             Objects  PC      Programs  Python         setup.py
ly@ubuntu:/opt/python35/Python-3.5.3$
```

解决 Python3.5 编译依赖

```bash
ly@ubuntu:~$ sudo aptitude install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget -y
```

Python3.5 编译

```bash
ly@ubuntu:~$ cd /opt/python35/Python-3.5.3/
ly@ubuntu:/opt/python35/Python-3.5.3$ sudo ./configure --enable-optimizations --prefix=/usr/local/python35
ly@ubuntu:/opt/python35/Python-3.5.3$ sudo make
ly@ubuntu:/opt/python35/Python-3.5.3$ sudo make install
```

测试

```bash
ly@ubuntu:~$ /usr/local/python35/bin/python3.5 -V
Python 3.5.3
ly@ubuntu:~$
```

卸载

```bash
ly@ubuntu:~$ sudo rm -rf /usr/local/python35/
```

## 通过 Docker 容器运行 Python 环境

在配置 Docker Python 之前需要说明一下，容器应用很广泛，功能异常强大，下面只能将环境搭建起来，涉及到的容器基础知识需要读者自行学习。

安装 Docker

```bash
ly@ubuntu:~$ sudo aptitude install docker.io -y
```

配置 Docker 国内加速源

```bash
ly@ubuntu:~$ sudo cat /etc/docker/daemon.json 
{
  "registry-mirrors": ["https://dqtef2wb.mirror.aliyuncs.com"]
}
ly@ubuntu:~$ sudo systemctl daemon-reload
ly@ubuntu:~$ sudo systemctl restart docker
```

+ 国内加速的网站有很多，而且多数网站都有自己的加速脚本，科学上网搜索即可

### 配置 Python2.7

拉取 python:2.7 镜像

```bash
ly@ubuntu:~$ sudo docker pull python:2.7
```

查看 python:2.7 镜像是否拉取成功

```bash
ly@ubuntu:~$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python              2.7                 805f1ed43956        3 weeks ago         842MB
python              3.5                 090e4d6705ae        3 weeks ago         853MB
python              3.6                 4567cdde50f8        3 weeks ago         856MB
ly@ubuntu:~$
```

运行 python:2.7 容器

```bash
ly@ubuntu:~$ sudo docker run -d -it python:2.7
29c023fcd172b77e54aede9d8c9cee4ada758f9964c698b4f02c17aa1b9758ce
ly@ubuntu:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
29c023fcd172        python:2.7          "python2"           6 seconds ago       Up 4 seconds                            priceless_mclaren
ly@ubuntu:~$
```

查看 python:2.7 容器是否运行成功

```bash
ly@ubuntu:~$ sudo docker exec -it priceless_mclaren python -V
Python 2.7.17
ly@ubuntu:~$ 
```

进入 python:2.7 容器测试

```bash
ly@ubuntu:~$ sudo docker exec -it priceless_mclaren bash
root@29c023fcd172:/# python
Python 2.7.17 (default, Feb  2 2020, 06:58:20) 
[GCC 8.3.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> print"hello world"
hello world
>>> 
```

删除容器 python:2.7

```bash
ly@ubuntu:~$ sudo docker rm -f priceless_mclaren
priceless_mclaren
ly@ubuntu:~$ 
```

### 配置 Python3.5

拉取 python:3.5 镜像

```bash
ly@ubuntu:~$ sudo docker pull python:3.5
```

查看 python:3.5 镜像是否拉取成功

```bash
ly@ubuntu:~$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python              2.7                 805f1ed43956        3 weeks ago         842MB
python              3.5                 090e4d6705ae        3 weeks ago         853MB
python              3.6                 4567cdde50f8        3 weeks ago         856MB
ly@ubuntu:~$
```

运行 python:3.5 容器

```bash
ly@ubuntu:~$ sudo docker run -d -it python:3.5
d9abe17e315aaeef779e1367091fce62c5ce8181773bbd37fe8b084f6e9f509e
ly@ubuntu:~$ 
ly@ubuntu:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
d9abe17e315a        python:3.5          "python3"           4 seconds ago       Up 3 seconds                            inspiring_goodall
ly@ubuntu:~$
```

查看 python:3.5 容器是否运行成功

```bash
ly@ubuntu:~$ sudo docker exec -it inspiring_goodall python -V
Python 3.5.9
ly@ubuntu:~$
```

进入 python:3.5 容器测试

```bash
ly@ubuntu:~$ sudo docker exec -it inspiring_goodall bash
root@d9abe17e315a:/# 
root@d9abe17e315a:/# python3
Python 3.5.9 (default, Feb  2 2020, 06:23:32) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> print("hello world")
hello world
>>> 
```

删除容器 python:3.5

```bash
ly@ubuntu:~$ sudo docker rm -f inspiring_goodall
inspiring_goodall
ly@ubuntu:~$
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
ly@ubuntu:~$ sudo aptitude install git -y
```

安装 Python 编译依赖

```bash
ly@ubuntu:~$ sudo aptitude install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev -y
```

创建用户 python

```bash
ly@ubuntu:~$ sudo adduser python
```

使用 python 用户登录后安装 Pyenv

```bash
ly@ubuntu:~$ sudo su - python
python@ubuntu:~$
python@ubuntu:~$ curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   148  100   148    0     0     24      0  0:00:06  0:00:06 --:--:--    40
100  2454  100  2454    0     0    359      0  0:00:06  0:00:06 --:--:-- 2396k
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
python@ubuntu:~$ 
```

默认安装到了当前用户的 ~/.pyenv 下面

```bash
python@ubuntu:~$ ls .pyenv/
bin  CHANGELOG.md  COMMANDS.md  completions  CONDUCT.md  libexec  LICENSE  Makefile  plugins  pyenv.d  README.md  src  terminal_output.png  test
python@ubuntu:~$
```

在 python 用户的 ~/.bash_profile 中追加

```bash
python@ubuntu:~$ cat .bash_profile 
# the following to ~/.bashrc:
export PATH="/home/python/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
python@ubuntu:~$ 
python@ubuntu:~$ source .bash_profile 
python@ubuntu:~$
```

- 这样当该用户启动的时候，会执行用户的 .bash_profile 中的脚本，就会启动 pyenv。安装好的 pyenv 就在 ~/.pyenv 中

### Pyenv 的使用

#### Python 命令

查看命令帮助

```bash
python@ubuntu:~$ pyenv --help
Usage: pyenv <command> [<args>]

Some useful pyenv commands are:
   activate    Activate virtual environment
   commands    List all available pyenv commands
   deactivate   Deactivate virtual environment
   doctor      Verify pyenv installation and development tools to build pythons.
   exec        Run an executable with the selected Python version
......
```

列出所有可用版本

```bash
python@ubuntu:~$ pyenv install --list
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
python@ubuntu:~$ pyenv install 3.5.3
Downloading Python-3.5.3.tar.xz...
-> https://www.python.org/ftp/python/3.5.3/Python-3.5.3.tar.xz
Installing Python-3.5.3...
WARNING: The Python bz2 extension was not compiled. Missing the bzip2 lib?
WARNING: The Python sqlite3 extension was not compiled. Missing the SQLite3 lib?
Installed Python-3.5.3 to /home/python/.pyenv/versions/3.5.3
python@ubuntu:~$ 
```

查看安装好的 Python

```bash
python@ubuntu:~$ ls .pyenv/versions/
3.5.3
python@ubuntu:~$ 
python@ubuntu:~$ pyenv versions
* system (set by /home/python/.pyenv/version)
  3.5.3
python@ubuntu:~$
```

#### Pyenv 的 Python 版本控制

Pyenv version 显示当前的 Python 版本，Pyenv versions 显示所有可用的 Python 版本和当前版本

```bash
python@ubuntu:~$ pyenv version
system (set by /home/python/.pyenv/version)
python@ubuntu:~$ 
python@ubuntu:~$ pyenv versions
* system (set by /home/python/.pyenv/version)
  3.5.3
python@ubuntu:~$
```

global 全局设置

```bash
python@ubuntu:~$ pyenv global 3.5.3
python@ubuntu:~$ 
python@ubuntu:~$ pyenv version
3.5.3 (set by /home/python/.pyenv/version)
python@ubuntu:~$ 
python@ubuntu:~$ pyenv versions
  system
* 3.5.3 (set by /home/python/.pyenv/version)
python@ubuntu:~$ 
```

+ 可以看到所有受 pyenv 控制的窗口中都是 3.5.3 的 python 版本了。
+ 这里用 global 是作用于非 root 用户 python 用户上，如果是 root 用户安装，请不要使用 global，否则影响太大。

shell 会话设置

```bash
python@ubuntu:~$ pyenv global system
python@ubuntu:~$ pyenv versions
* system (set by /home/python/.pyenv/version)
  3.5.3
python@ubuntu:~$ 
python@ubuntu:~$ pyenv shell 3.5.3
python@ubuntu:~$ 
python@ubuntu:~$ pyenv versions
  system
* 3.5.3 (set by PYENV_VERSION environment variable)
python@ubuntu:~$
```

+ 影响只作用于当前会话

local 本地设置

```bash
python@ubuntu:~$ pyenv local 3.5.3
```

+ 使用 pyenv local 设置从当前工作目录开始向下递归都继承这个设置

#### Virtualenv 虚拟环境设置

> 为什么要使用虚拟环境？
>
> 因为刚才使用的 Python 环境都是一个公共的空间，如果多个项目使用不同 Python 版本开发，或者使用不同的 Python 版本部署运行，或者使用同样的版本开发的但不同项目使用了不同版本的库，等等这些问题都会带来冲突。最好的解决办法就是每一个项目独立运行自己的独立小环境中。

使用插件，在 plugins/pyenv-virtualenv 中

```bash
python@ubuntu:~$ pyenv virtualenv 3.5.3 cec353
Requirement already satisfied: setuptools in /home/python/.pyenv/versions/3.5.3/envs/cec353/lib/python3.5/site-packages
Requirement already satisfied: pip in /home/python/.pyenv/versions/3.5.3/envs/cec353/lib/python3.5/site-packages
python@ubuntu:~$
```

使用 Python3.5.3 版本创建出了一个独立的虚拟空间

```bash
python@ubuntu:~$ pyenv versions
  3.5.3
  3.5.3/envs/cec353
  cec353
python@ubuntu:~$ 
```

+ 可见在版本列表中存在，就和 3.5.3 是一样的，就是一个版本了
+ 真实目录在 ~/.pyenv/versions/ 下，以后只要使用这个虚拟版本，包就会安装到这些对应的目录下面，而不是使用 3.5.3

```bash
python@ubuntu:~$ mkdir -p cec/greatwall/PK
python@ubuntu:~$ cd cec/greatwall/PK
python@ubuntu:~/cec/greatwall/PK$ 
python@ubuntu:~/cec/greatwall/PK$ pyenv local cec353
python@ubuntu:~/cec/greatwall/PK$ pyenv versions
  3.5.3
  3.5.3/envs/cec353
* cec353 (set by /home/python/cec/greatwall/PK/.python-version)
python@ubuntu:~/cec/greatwall/PK$ 
```

## Python 包管理工具 Pip

Pip 是 Python 的包管理工具，3.x 的版本直接带了，可以直接使用。

安装 Pip2

```bash
ly@ubuntu:~$ sudo aptitude install python-pip -y
ly@ubuntu:~$ sudo pip --version
pip 9.0.1 from /usr/lib/python2.7/dist-packages (python 2.7)
ly@ubuntu:~$
```

安装 Pip3

```bash
ly@ubuntu:~$ sudo aptitude install python3-pip -y
ly@ubuntu:~$ sudo pip3 --version
pip 9.0.1 from /usr/lib/python3/dist-packages (python 3.6)
ly@ubuntu:~$
```

配置国内的 Pip 源

```bash
ly@ubuntu:~$ mkdir ~/.pip
ly@ubuntu:~$ vim .pip/pip.conf
ly@ubuntu:~$ cat .pip/pip.conf 
[global]
index-url=https://mirrors.aliyun.com/pypi/simple/
trusted-host=mirrors.aliyun.com
ly@ubuntu:~$ 
```

安装 redis 测试

```bash
ly@ubuntu:~$ pip3 install redis
Collecting redis
  Downloading https://mirrors.aliyun.com/pypi/packages/f0/05/1fc7feedc19c123e7a95cfc9e7892eb6cdd2e5df4e9e8af6384349c1cc3d/redis-3.4.1-py2.py3-none-any.whl (71kB)
    100% |████████████████████████████████| 71kB 8.5MB/s 
Installing collected packages: redis
Successfully installed redis
You are using pip version 8.1.1, however version 20.0.2 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
ly@ubuntu:~$
```

安装 ipython 测试

```bash
ly@ubuntu:~$ pip3 install ipython
ly@ubuntu:~$ .local/bin/ipython
Python 3.5.2 (default, Dec  1 2017, 03:38:03) 
Type 'copyright', 'credits' or 'license' for more information
IPython 7.9.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: print("hello world")                                                                                                                                 
hello world

In [2]:
```


