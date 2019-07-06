# Pyenv安装
## 操作系统准备
> + 准备Linux最小系统即可。   
> + 如果在虚拟机中克隆，MAC地址会变。 这里使用CentOS 6.5+   

## pyenv安装方式
### git 安装
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
```
> **注意：**   
> > 1. 在 https://github.com/pyenv/pyenv-installer 有安装文档   
> > 2. 如果curl出现 curl: (35) SSL connect error ，是nss版本低的问题，更新它。 可能需要配置一个有较新包的yum源   

```bash
[updates]
name=CentOS-Updates
baseurl=https://mirrors.aliyun.com/centos/6.9/os/x86_64
gpgcheck=0
```
> > 3. 然后更新nss # yum update nss   

> 在python用户的~/.bash_profile中追加   

```bash
export PATH="/home/python/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
$ source ~/.bash_profile
```

+ 这样当用户启动的时候，会执行用户的.bash_profile中的脚本，就会启动pyenv。 安装好的pyenv就在~/.pyenv中

# Pyenv的使用
## python 版本及path路径   

```bash
$ python --version
$ python -V
$ echo $PATH
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

```bash
列出所有可用版本
$ pyenv install --list
在线安装指定版本
$ pyenv install 3.5.3
$ pyenv versions
这样的安装可能较慢，为了提速，可是选用cache方法。   

使用缓存方式安装 在~/.pyenv目录下，新建cache目录，放入下载好的待安装版本的文件。 不确定要哪一个文件，把下载的3个文件都放进去。   
$ pyenv install 3.5.3 -v
```




