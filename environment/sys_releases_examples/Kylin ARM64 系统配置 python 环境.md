# Kylin ARM64 系统配置 Python

在配置 Python 环境之前，首先列举一下底层环境，包括硬件和操作系统

+ 服务器是飞腾 2000+，CPU 是 ARM64 架构
+ 操作系统是 ARM64 版银河麒麟操作系统，使用方式跟 Ubuntu 类似

## 通过 APT 源安装 Python

### 配置 Ubuntu ARM64 源

```bash
greatwall@Kylin:~$ sudo cat /etc/apt/sources.list
# deb http://archive.kylinos.cn/kylin/KYLIN-ALL 4.0.2sp2-server main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe
greatwall@Kylin:~$ 
```

+ 麒麟系统 apt 源配置了两个，kylin 和 ubuntu 源，需要注意的是，两个源配置在一起有可能会出现包冲突，最好的解决办法是用 aptitude 命令进行安装，一旦有冲突发生，这个命令会出示解决方案，管理员可以酌情安装

### Python2.7 安装

安装

```bash
greatwall@Kylin:~$ sudo aptitude install python2.7 -y
greatwall@Kylin:~$
```

测试

```bash
greatwall@Kylin:~$ sudo python2.7
Python 2.7.12 (default, Oct  8 2019, 14:14:10) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> print("hello world")
hello world
>>> 
```

卸载

```bash
greatwall@Kylin:~$ sudo aptitude purge python2.7 -y
```

### Python3.5 安装

安装

```bash
greatwall@Kylin:~$ sudo aptitude install python3.5 -y
greatwall@Kylin:~$
```

测试

```bash
greatwall@Kylin:~$ sudo python3.5
Python 3.5.2 (default, Dec  1 2017, 03:38:03) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print("hello world")
hello world
>>> 
```

卸载

```bash
greatwall@Kylin:~$ sudo aptitude purge python3.5 -y
```

## 通过 Python 源码编译安装

### Python2.7 安装

源码下载

```bash
greatwall@Kylin:~$ sudo wget https://www.python.org/ftp/python/2.7.16/Python-2.7.16.tar.xz
greatwall@Kylin:~$ sudo ls
Python-2.7.16.tar.xz
greatwall@Kylin:~$
```

源码解压到指定的目录

```bash
greatwall@Kylin:~$ sudo ls
Python-2.7.16.tar.xz
greatwall@Kylin:~$ sudo mkdir /opt/python27
greatwall@Kylin:~$ sudo tar xf Python-2.7.16.tar.xz -C /opt/python27
greatwall@Kylin:~$ sudo cd /opt/python27
greatwall@Kylin:/opt/python27$ sudo ls
Python-2.7.16
greatwall@Kylin:/opt/python27$ sudo cd Python-2.7.16/
greatwall@Kylin:/opt/python27/Python-2.7.16$ sudo ls
aclocal.m4    config.sub  configure.ac  Doc      Include     Lib      Mac              Misc     Objects  PC       pyconfig.h.in  README  setup.py
config.guess  configure   Demo          Grammar  install-sh  LICENSE  Makefile.pre.in  Modules  Parser   PCbuild  Python         RISCOS  Tools
greatwall@Kylin:/opt/python27/Python-2.7.16$
```

解决 Python2.7 编译依赖

```bash
greatwall@Kylin:/opt/python27/Python-2.7.16$ sudo aptitude install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget
     ......
     降级 下列软件包：                                       
1)     libffi6 [3.2.1-4kord (now) -> 3.2.1-4 (xenial)]       
2)     libgdbm3 [1.8.3-13.1kord (now) -> 1.8.3-13.1 (xenial)]
是否接受该解决方案？[Y/n/q/?] y
```

Python2.7 编译

```bash
greatwall@Kylin:/opt/python27/Python-2.7.16$ sudo mkdir /usr/local/python27
greatwall@Kylin:/opt/python27/Python-2.7.16$ sudo ./configure --enable-optimizations --prefix=/usr/local/python27
greatwall@Kylin:/opt/python27/Python-2.7.16$ sudo make && make install 
```

测试

```bash
greatwall@Kylin:/opt/python27/Python-2.7.16$ sudo cd /usr/local/python27/
greatwall@Kylin:/usr/local/python27$ sudo ./bin/python2 -V
Python 2.7.16
greatwall@Kylin:/usr/local/python27$
```

卸载

```bash
greatwall@Kylin:~$ sudo rm -rf /usr/local/python27
```

### Python3.5 安装

源码下载

```bash
greatwall@Kylin:~$ sudo wget https://www.python.org/ftp/python/3.5.3/Python-3.5.3.tar.xz
greatwall@Kylin:~$
```

源码解压到指定的目录

```bash
greatwall@Kylin:~$ sudo ls
Python-3.5.3.tar.xz
greatwall@Kylin:~$ sudo tar xf Python-3.5.3.tar.xz -C /opt/python35/
greatwall@Kylin:~$  
greatwall@Kylin:~$ sudo ls /opt/python35/
Python-3.5.3
greatwall@Kylin:~$ sudo cd /opt/python35/Python-3.5.3/
greatwall@Kylin:/opt/python35/Python-3.5.3$ 
greatwall@Kylin:/opt/python35/Python-3.5.3$ sudo ls
aclocal.m4    config.sub  configure.ac  Grammar  install-sh  LICENSE  Makefile.pre.in  Modules  Parser  PCbuild   pyconfig.h.in  README    Tools
config.guess  configure   Doc           Include  Lib         Mac      Misc             Objects  PC      Programs  Python         setup.py
greatwall@Kylin:/opt/python35/Python-3.5.3$ 
```

解决 Python3.5 编译依赖

```bash
greatwall@Kylin:~$ sudo aptitude install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget
	......
     降级 下列软件包：                                       
1)     libffi6 [3.2.1-4kord (now) -> 3.2.1-4 (xenial)]       
2)     libgdbm3 [1.8.3-13.1kord (now) -> 1.8.3-13.1 (xenial)]
是否接受该解决方案？[Y/n/q/?] y
```

Python3.5 编译

```bash
greatwall@Kylin:/opt/python35/Python-3.5.3$ sudo ./configure --enable-optimizations --prefix=/usr/local/python35
greatwall@Kylin:/opt/python35/Python-3.5.3$ sudo make
greatwall@Kylin:/opt/python35/Python-3.5.3$ sudo make install
```

测试

```bash
greatwall@Kylin:/opt/python35/Python-3.5.3$ sudo /usr/local/python35/bin/python3.5 -V
Python 3.5.3
greatwall@Kylin:/opt/python35/Python-3.5.3$
```

卸载

```bash
greatwall@Kylin:~$ sudo rm -rf /usr/local/python35
```

## 通过 Docker 容器运行 Python 环境

在配置 Docker Python 之前需要说明一下，容器应用很广泛，功能异常强大，下面只能将环境搭建起来，涉及到的容器基础知识需要读者自行学习。

安装 Docker

```bash
greatwall@Kylin:~$ sudo aptitude install docker.io -y
greatwall@Kylin:~$
```

配置 Docker 国内加速源

```bash
greatwall@Kylin:~$ sudo cat /etc/docker/daemon.json 
{
  "registry-mirrors": ["https://dqtef2wb.mirror.aliyuncs.com"]
}
greatwall@Kylin:~$
```

+ 国内加速的网站有很多，而且多数网站都有自己的加速脚本，科学上网搜索即可

### 配置 Python2.7

拉取 python:2.7 镜像

```bash
greatwall@Kylin:~$ sudo docker pull python:2.7
2.7: Pulling from library/python
Digest: sha256:b104660eb48e8b71148f670a9404ede4211cdc16ef33fad3e01be84d4e4817ec
Status: Image is up to date for python:2.7
greatwall@Kylin:~$
```

查看 python:2.7 镜像是否拉取成功

```bash
greatwall@Kylin:~$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python              2.7                 805f1ed43956        2 weeks ago         842MB
greatwall@Kylin:~$ 
```

运行 python:2.7 容器

```bash
greatwall@Kylin:~$ sudo docker run -d -it python:2.7
6ef128ed770b905550b3147db8628bbd7d62ee1c9fd13c649e194a283b8b4bb5
greatwall@Kylin:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
6ef128ed770b        python:2.7          "python2"           8 seconds ago       Up 7 seconds                            vibrant_borg
greatwall@Kylin:~$ 
```

查看 python:2.7 容器是否运行成功

```bash
greatwall@Kylin:~$ sudo docker exec -it vibrant_borg python -V
Python 2.7.17
greatwall@Kylin:~$
```

进入 python:2.7 容器测试

```bash
greatwall@Kylin:~$ sudo docker run -d -it -v /tmp/:/tmp/ python:2.7
e873f5d236b5e5f05aa3fff4b30b8d76660ef43256054d6b2645d8257317d325
greatwall@Kylin:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
e873f5d236b5        python:2.7          "python2"           39 seconds ago      Up 38 seconds                           jovial_nash
04aafb50f462        python:3.5          "python3"           About an hour ago   Up About an hour                        romantic_burnell
6ef128ed770b        python:2.7          "python2"           About an hour ago   Up About an hour                        vibrant_borg
greatwall@Kylin:~$ sudo docker exec -it jovial_nash bash
root@e873f5d236b5:/# python /tmp/99.py 
1 * 1 = 1 
1 * 2 = 2  2 * 2 = 4 
1 * 3 = 3  2 * 3 = 6  3 * 3 = 9 
1 * 4 = 4  2 * 4 = 8  3 * 4 = 12 4 * 4 = 16
1 * 5 = 5  2 * 5 = 10 3 * 5 = 15 4 * 5 = 20 5 * 5 = 25
1 * 6 = 6  2 * 6 = 12 3 * 6 = 18 4 * 6 = 24 5 * 6 = 30 6 * 6 = 36
1 * 7 = 7  2 * 7 = 14 3 * 7 = 21 4 * 7 = 28 5 * 7 = 35 6 * 7 = 42 7 * 7 = 49
1 * 8 = 8  2 * 8 = 16 3 * 8 = 24 4 * 8 = 32 5 * 8 = 40 6 * 8 = 48 7 * 8 = 56 8 * 8 = 64
1 * 9 = 9  2 * 9 = 18 3 * 9 = 27 4 * 9 = 36 5 * 9 = 45 6 * 9 = 54 7 * 9 = 63 8 * 9 = 72 9 * 9 = 81
root@e873f5d236b5:/#
```

删除容器 python:2.7

```bash
greatwall@Kylin:~$ sudo docker rm -f jovial_nash
jovial_nash
greatwall@Kylin:~$
```

### 配置 Python3.5

拉取 python:3.5 镜像

```bash
greatwall@Kylin:~$ sudo docker pull python:3.5
3.5: Pulling from library/python
Digest: sha256:24f39a9542911c29dc84b3c931b08735fb79562a8fbbc97339e41e4d5bb126f0
Status: Image is up to date for python:3.5
greatwall@Kylin:~$ 
```

查看 python:3.5 镜像是否拉取成功

```bash
greatwall@Kylin:~$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python              2.7                 805f1ed43956        2 weeks ago         842MB
python              3.5                 090e4d6705ae        2 weeks ago         853MB
python              3.6                 4567cdde50f8        2 weeks ago         856MB
greatwall@Kylin:~$ 
```

运行 python:3.5 容器

```bash
greatwall@Kylin:~$ sudo docker run -d -it python:3.5 
04aafb50f462a4aee115298d72c46943b5c5f0c9d46d88e5f0e5a7ded1985699
greatwall@Kylin:~$ 
greatwall@Kylin:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
04aafb50f462        python:3.5          "python3"           15 seconds ago      Up 14 seconds                           romantic_burnell
6ef128ed770b        python:2.7          "python2"           5 minutes ago       Up 5 minutes                            vibrant_borg
greatwall@Kylin:~$ 
```

查看 python:3.5 容器是否运行成功

```bash
greatwall@Kylin:~$ sudo docker exec -it romantic_burnell python -V
Python 3.5.9
greatwall@Kylin:~$
```

进入 python:3.5 容器测试

```bash
greatwall@Kylin:~$ sudo docker run -it -d -v /tmp/:/tmp/ python:3.5
76bffc703b965eaf2380b59d131a0867135dde128388357203027c7e8e8d07d6
greatwall@Kylin:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
76bffc703b96        python:3.5          "python3"           7 seconds ago       Up 5 seconds                            boring_haibt
greatwall@Kylin:~$ sudo docker exec -it boring_haibt bash
root@76bffc703b96:/# python /tmp/99.py 
1 * 1 =  1 
1 * 2 =  2 2 * 2 =  4 
1 * 3 =  3 2 * 3 =  6 3 * 3 =  9 
1 * 4 =  4 2 * 4 =  8 3 * 4 = 12 4 * 4 = 16 
1 * 5 =  5 2 * 5 = 10 3 * 5 = 15 4 * 5 = 20 5 * 5 = 25 
1 * 6 =  6 2 * 6 = 12 3 * 6 = 18 4 * 6 = 24 5 * 6 = 30 6 * 6 = 36 
1 * 7 =  7 2 * 7 = 14 3 * 7 = 21 4 * 7 = 28 5 * 7 = 35 6 * 7 = 42 7 * 7 = 49 
1 * 8 =  8 2 * 8 = 16 3 * 8 = 24 4 * 8 = 32 5 * 8 = 40 6 * 8 = 48 7 * 8 = 56 8 * 8 = 64 
1 * 9 =  9 2 * 9 = 18 3 * 9 = 27 4 * 9 = 36 5 * 9 = 45 6 * 9 = 54 7 * 9 = 63 8 * 9 = 72 9 * 9 = 81 
root@76bffc703b96:/#
```

删除容器 python:3.5

```bash
greatwall@Kylin:~$ sudo docker rm -f boring_haibt
boring_haibt
greatwall@Kylin:~$
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
greatwall@Kylin:~$ sudo aptitude install git -y
greatwall@Kylin:~$ 
```

安装 Python 编译依赖

```bash
greatwall@Kylin:~$ sudo aptitude install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev -y
```

创建用户 python

```bash
greatwall@Kylin:~$ sudo useradd python
```

使用 python 用户登录后安装 Pyenv

```bash
greatwall@Kylin:~$ sudo su - python
$
$ curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   148  100   148    0     0    146      0  0:00:01  0:00:01 --:--:--   146
100  2454  100  2454    0     0   1145      0  0:00:02  0:00:02 --:--:--  3901
正克隆到 '/home/python/.pyenv'...
......
正克隆到 '/home/python/.pyenv/plugins/pyenv-doctor'...
......
正克隆到 '/home/python/.pyenv/plugins/pyenv-installer'...
......
正克隆到 '/home/python/.pyenv/plugins/pyenv-update'...
......
正克隆到 '/home/python/.pyenv/plugins/pyenv-virtualenv'...
......
正克隆到 '/home/python/.pyenv/plugins/pyenv-which-ext'...
......
WARNING: seems you still have not added 'pyenv' to the load path.
# Load pyenv automatically by adding
# the following to ~/.bashrc:
export PATH="/home/python/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
$ 
```

默认安装到了当前用户的 ~/.pyenv 下面

```bash
$ ls .pyenv/
bin  CHANGELOG.md  COMMANDS.md	completions  CONDUCT.md  libexec  LICENSE  Makefile  plugins  pyenv.d  README.md  src  terminal_output.png  test
$ 
```

在 python 用户的 ~/.bash_profile 中追加

```bash
$ cat .bash_profile 
export PATH="/home/python/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
$ 
$ source .bash_profile
$ 
```

- 这样当该用户启动的时候，会执行用户的 .bash_profile 中的脚本，就会启动 pyenv。安装好的 pyenv 就在 ~/.pyenv 中

### Pyenv 的使用

#### Python 命令

查看命令帮助

```bash
$ pyenv --help
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
$ pyenv install --list
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
$ pyenv install 3.5.3
Downloading Python-3.5.3.tar.xz...
-> https://www.python.org/ftp/python/3.5.3/Python-3.5.3.tar.xz
Installing Python-3.5.3...
WARNING: The Python bz2 extension was not compiled. Missing the bzip2 lib?
WARNING: The Python sqlite3 extension was not compiled. Missing the SQLite3 lib?
Installed Python-3.5.3 to /home/python/.pyenv/versions/3.5.3
$ 
```

查看安装好的 Python

```bash
$ ls .pyenv/versions/
3.5.3
$ 
$ pyenv versions
* system (set by /home/python/.pyenv/version)
  3.5.3
$
```

#### Pyenv 的 Python 版本控制

Pyenv version 显示当前的 Python 版本，Pyenv versions 显示所有可用的 Python 版本和当前版本

```bash
$ pyenv version
system (set by /home/python/.pyenv/version)
$ 
$ pyenv versions
* system (set by /home/python/.pyenv/version)
  3.5.3
$
```

global 全局设置

```bash
$ pyenv global 3.5.3
$ 
$ pyenv version
3.5.3 (set by /home/python/.pyenv/version)
$ 
$ pyenv versions
  system
* 3.5.3 (set by /home/python/.pyenv/version)
$ 
```

+ 可以看到所有受 pyenv 控制的窗口中都是 3.5.3 的 python 版本了。
+ 这里用 global 是作用于非 root 用户 python 用户上，如果是 root 用户安装，请不要使用 global，否则影响太大。

shell 会话设置

```bash
$ pyenv global system
$ pyenv versions
* system (set by /home/python/.pyenv/version)
  3.5.3
$ 
$ pyenv shell 3.5.3
$ 
$ pyenv versions
  system
* 3.5.3 (set by PYENV_VERSION environment variable)
$
```

+ 影响只作用于当前会话

local 本地设置

```bash
$ pyenv local 3.5.3
```

+ 使用 pyenv local 设置从当前工作目录开始向下递归都继承这个设置

#### Virtualenv 虚拟环境设置

> 为什么要使用虚拟环境？
>
> 因为刚才使用的 Python 环境都是一个公共的空间，如果多个项目使用不同 Python 版本开发，或者使用不同的 Python 版本部署运行，或者使用同样的版本开发的但不同项目使用了不同版本的库，等等这些问题都会带来冲突。最好的解决办法就是每一个项目独立运行自己的独立小环境中。

使用插件，在 plugins/pyenv-virtualenv 中

```bash
$ pyenv virtualenv 3.5.3 cec353
Requirement already satisfied: setuptools in /home/python/.pyenv/versions/3.5.3/envs/cec353/lib/python3.5/site-packages
Requirement already satisfied: pip in /home/python/.pyenv/versions/3.5.3/envs/cec353/lib/python3.5/site-packages
$ 
```

使用 Python3.5.3 版本创建出了一个独立的虚拟空间

```bash
$ pyenv versions
  system
* 3.5.3 (set by PYENV_VERSION environment variable)
  3.5.3/envs/cec353
  cec353
$ 
```

+ 可见在版本列表中存在，就和 3.5.3 是一样的，就是一个版本了
+ 真实目录在 ~/.pyenv/versions/ 下，以后只要使用这个虚拟版本，包就会安装到这些对应的目录下面，而不是使用 3.5.3

```bash
$ mkdir -p cec/greatwall/PK
$ ls cec/greatwall/
PK
$ cd cec/greatwall/PK/
$ pwd
/home/python/cec/greatwall/PK
$ 
$ pyenv local cec353
$ pyenv versions
  ......
  3.5.3/envs/cec353
* cec353 (set by /home/python/cec/greatwall/PK/.python-version)
```

## Python 包管理工具 Pip

Pip 是 Python 的包管理工具，3.x 的版本直接带了，可以直接使用。

安装 Pip2

```bash
greatwall@Kylin:~$ sudo aptitude install python-pip
下列“新”软件包将被安装。         
  python-pip python-pip-whl{a} 
下列软件包被“推荐”安装但是将“不会”被安装：
  python-all-dev python-setuptools python-wheel 
0 个软件包被升级，新安装 2 个， 0 个将被删除， 同时 163 个将不升级。
需要获取 1,255 kB 的存档。 解包后将要使用 1,854 kB。
您要继续吗？[Y/n/?] y
......
greatwall@Kylin:~$ sudo pip
pip   pip2  
greatwall@Kylin:~$ sudo pip --version
pip 8.1.1 from /usr/lib/python2.7/dist-packages (python 2.7)
greatwall@Kylin:~$
```

安装 Pip3

```bash
greatwall@Kylin:~$ sudo aptitude install python3-pip
下列“新”软件包将被安装。         
  python-pip-whl{a} python3-pip 
下列软件包被“推荐”安装但是将“不会”被安装：
  python3-dev python3-setuptools python3-wheel 
0 个软件包被升级，新安装 2 个， 0 个将被删除， 同时 163 个将不升级。
需要获取 109 kB/1,219 kB 的存档。 解包后将要使用 1,789 kB。
您要继续吗？[Y/n/?] y
......
greatwall@Kylin:~$ sudo pip3 --version
pip 8.1.1 from /usr/lib/python3/dist-packages (python 3.5)
greatwall@Kylin:~$
```

配置国内的 Pip 源

```bash
$ mkdir ~/.pip
$ vim .pip/pip.conf
$ cat .pip/pip.conf 
[global]
index-url=https://mirrors.aliyun.com/pypi/simple/
trusted-host=mirrors.aliyun.com
$ 
```

安装 redis 测试

```bash
$ pip3 install redis
Collecting redis
  Downloading https://mirrors.aliyun.com/pypi/packages/f0/05/1fc7feedc19c123e7a95cfc9e7892eb6cdd2e5df4e9e8af6384349c1cc3d/redis-3.4.1-py2.py3-none-any.whl (71kB)
    100% |████████████████████████████████| 71kB 8.5MB/s 
Installing collected packages: redis
Successfully installed redis
You are using pip version 8.1.1, however version 20.0.2 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
$
```

安装 ipython 测试

```bash
$ pip3 install ipython
$ .local/bin/ipython
Python 3.5.2 (default, Dec  1 2017, 03:38:03) 
Type 'copyright', 'credits' or 'license' for more information
IPython 7.9.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: print("hello world")                                                                                                                                 
hello world

In [2]:
```


