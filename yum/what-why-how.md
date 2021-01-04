# yum介绍

## `yum`是什么？

`yum`（ `Yellow dog Updater, Modified`）是一个在`Fedora`和`RedHat`以及`SUSE`中的`Shell`前端软件包管理器。

`yum`基於`RPM`包管理，能够从指定的`yum`服务器自动下载`RPM`包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。

## `yum`为什么而存在？

说到`yum`源就必须说到`linux`系统中特有的依赖关系问题，`yum`就是为了解决依赖关系而存在的。`yum`源就相当是一个目录项，当我们使用`yum`机制安装软件时，若需要安装依赖软件，则`yum`机制就会根据在`yum`源中定义好的路径查找依赖软件，并将依赖软件安装好。

## `yum`的工作原理

`yum`采用的是`client/server`模式, `yum`的工作需要两部分来合作，一部分是`yum`服务器，还有就是`client`的`yum`工具。

### `yum`服务器

服务器也就是`yum`仓库，存放着`rmp`包、元数据`metadata`等信息。（元数据整理出每个`rpm`包的基本信息，包括`rpm`包对应的版本号，`conf`文件，`binary`信息，以及很关键的依赖信息）

仓库里面最重要的两个目录
* `/repodata`：存放`metadata`文件
* `/Packges`：存放`rpm`包

#### 搭建`yum`服务器

### `yum`客户端

当客户端要安装软件时，会按照客户端配置文件里指定的服务器路径，去访问`yum`服务器，并下载`metadata`缓存到客户端。然后根据`metadata`确认软件包的版本号，所需要的依赖包等，然后再去`yum`服务器一次下载安装所需要的`rpm`包。

#### 客户端配置文件

##### `/etc/yum.conf`

此文件提供了`yum`公共配置，定义了`yum`客户端基本的信息：

```text
$ cat /etc/yum.conf
[main]
cachedir=/var/cache/yum/$basearch/$releasever # 指定元数据缓存路径
keepcache=0 # 安装后是否保留客户端的rpm包
debuglevel=2
logfile=/var/log/yum.log # 指定日志路径
exactarch=1
obsoletes=1
gpgcheck=1 # 是否进行gpg验证
plugins=1
installonly_limit=5
bugtracker_url=http://bugs.centos.org/set_project.php?project_id=23&ref=http://bugs.centos.org/bug_report_page.php?category=yum
distroverpkg=centos-release
```

##### `/etc/yum.repos.d/*.repo`

`yum`仓库配置文件都存放在`/etc/yum.repos.d/`目录下，并且以`.repo`为文件名尾缀。这些文件指定了具体的仓库名称、路径等信息

```bash
$ cat /etc/yum.repos.d/CentOS-Base.repo
[base]
name=CentOS-$releasever - Base  #仓库名
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/ #仓库路径
enabled：1  #是否使能该仓库 
gpgcheck=1 #是否坚持gpg
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 #gpgkey路径
```