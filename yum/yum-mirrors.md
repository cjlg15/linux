# yum国内镜像源

有时候`CentOS`默认的`yum`源不一定是国内镜像，导致`yum`在线安装及更新速度不是很理想。这时候需要将`yum`源设置为国内镜像站点。国内主要开源的开源镜像站点应该是网易和阿里云了。

## 首先备份系统自带`yum`源配置文件`/etc/yum.repos.d/CentOS-Base.repo`

```bash
$ mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

## 下载国内`yum`源配置文件到本地

### CentOS7

```bash
$ cd /etc/yum.repos.d
# 网易
$ wget http://mirrors.163.com/.help/CentOS7-Base-163.repo -O CentOS7-Base-163.repo
# 阿里
$ wget -O /etc/yum.repos.d/CentOS7-Base-ali.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

上面分别下载了网易镜像源配置文件`CentOS7-Base-163.repo`和阿里源配置文件`CentOS7-Base-ali.repo`到本地`/etc/yum.repos.d`目录。

如果我们要使用网易镜像源，则直接重命令或复制`CentOS7-Base-163.repo`文件为`CentOS-Base.repo`即可。

```bash
$ cp CentOS7-Base-163.repo CentOS-Base.repo
```

## 更新缓存

```bash
$ yum makecache
```

## 安装或更新软件吧

享受飞一般的感觉。。。

```bash
$ yum -y install git
```