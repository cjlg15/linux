# CentOS 7 如何配置静态IP地址

Network Manager is a dynamic network control and configuration system that attempts to keep network devices and connections up and active when they are available). CentOS/RHEL 7 comes with Network Manager service installed and enabled by default.

> NetworkManager.conf是NetworkManager的配置文件。 它用于设置NetworkManager行为的各个方面。 主要的位置文件和配置目录可以通过使用--config，--config-dir，--system-config-dir和--intern-config参数来改变NetworkManager，分别。

验证NetworkManager服务的状态：

```bash
$ systemctl status NetworkManager.service
● NetworkManager.service - Network Manager
   Loaded: loaded (/usr/lib/systemd/system/NetworkManager.service; enabled; vendor preset: enabled)
   Active: active (running) since 日 2018-05-20 02:36:24 EDT; 41min ago
     Docs: man:NetworkManager(8)
 Main PID: 654 (NetworkManager)
   CGroup: /system.slice/NetworkManager.service
           └─654 /usr/sbin/NetworkManager --no-daemon

5月 20 02:45:06 localhost.localdomain NetworkManager[654]: <info>  [1526798706.7498] manager: NetworkManager state is now CONNECTING
5月 20 02:45:06 localhost.localdomain NetworkManager[654]: <info>  [1526798706.7500] device (enp0s3): state change: prepare -> config (reason 'none') [40 50 0]
5月 20 02:45:06 localhost.localdomain NetworkManager[654]: <info>  [1526798706.7685] device (enp0s3): state change: config -> ip-config (reason 'none') [50 70 0]
5月 20 02:45:06 localhost.localdomain NetworkManager[654]: <info>  [1526798706.7757] device (enp0s3): state change: ip-config -> ip-check (reason 'none') [70 80 0]
5月 20 02:45:06 localhost.localdomain NetworkManager[654]: <info>  [1526798706.7763] device (enp0s3): state change: ip-check -> secondaries (reason 'none') [80 90 0]
5月 20 02:45:06 localhost.localdomain NetworkManager[654]: <info>  [1526798706.7765] device (enp0s3): state change: secondaries -> activated (reason 'none') [90 100 0]
5月 20 02:45:06 localhost.localdomain NetworkManager[654]: <info>  [1526798706.7766] manager: NetworkManager state is now CONNECTED_LOCAL
5月 20 02:45:06 localhost.localdomain NetworkManager[654]: <info>  [1526798706.7832] manager: NetworkManager state is now CONNECTED_GLOBAL
5月 20 02:45:06 localhost.localdomain NetworkManager[654]: <info>  [1526798706.7833] policy: set 'enp0s3' (enp0s3) as default for IPv4 routing and DNS
5月 20 02:45:06 localhost.localdomain NetworkManager[654]: <info>  [1526798706.7834] device (enp0s3): Activation: successful, device activated.
```

要检查网络管理器管理哪个网络接口，请运行：

```
# nmcli dev status
设备    类型      状态    连接
enp0s3  ethernet  连接的  enp0s3
lo      loopback  未托管  --
```

有两种办法设置静态IP地址：

## 1. Configure a Static IP Address without Network Manager

1. 进入 `/etc/sysconfig/network-scripts`目录，找到网络接口文件，如`ifcfg-enp0s3`文件。
2. 编辑此文件:

```
TYPE=Ethernet
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
PROXY_METHOD=none
#BOOTPROTO=dhcp
#DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=ede5c01f-4d9e-451e-a5d3-52829ececdfb
DEVICE=enp0s3
# static assignment
ONBOOT=yes
BOOTPROTO=static # 设置gedy静态IP
IPADDR=192.168.3.200 # IPV4地址
NETMASK=255.255.252.0 # 子网掩码
GATEWAY=192.168.1.1 # 默认网关
NM_CONTROLLED=no # 关键是这个选项，

#BROADCAST=192.168.3.255
```
在上文中，`NM_CONTROLLED = no`表示将使用此配置文件设置此接口，而不是由Network Manager服务管理。
3. 保存配置并重启网络服务：`systemctl restart network.service`
4. 使用`ip add`查看网卡
5. 

## 二、使用传统方法，修改`/etc/resolv.conf`

1. 首先修改`NetworkManager`的配置，避免其动态覆盖`/etc/resolv.conf`配置文件。

```bash
$ vi /etc/NetworkManager/NetworkManager.conf

```

在main部分添加 “dns=none” 选项：

```bash
[main]
plugins=ifcfg-rh
dns=none
```

2. 重启NetworkManager

```
# systemctl restart NetworkManager.service
```
3. 手工修改 `# vi /etc/resolv.conf`

```bash
nameserver 114.114.114.114
nameserver 223.6.6.6
```


# 参考
* http://ask.xmodulo.com/configure-static-ip-address-centos7.html
* http://www.pubyun.com/blog/announce/centos-7-%E4%B8%8B%EF%BC%8C%E5%A6%82%E4%BD%95%E8%AE%BE%E7%BD%AEdns%E6%9C%8D%E5%8A%A1%E5%99%A8/
* https://blog.csdn.net/johnnycode/article/details/50184073
* https://wiki.centos.org/FAQ/CentOS7#head-a21a9e454157700367c9b7e9ccb1ff9954bec881