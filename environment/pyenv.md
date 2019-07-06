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


