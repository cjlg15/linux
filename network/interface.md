# 网络接口配置

在`Linux`中，不管是物理网卡还是虚拟网卡，都会在`/etc/sysconfig/network-scripts`目录下有一个`ifcfg-interface`的关联配置文件，其中`interface`为网卡名称，如：

```bash
# cd /etc/sysconfig/network-scripts
# ls ifcfg-*
ifcfg-eth0  ifcfg-eth1  ifcfg-lo
```

在上面这个例子中，系统有两个网卡配置文件：`ifcfg-eth0`和`ifcfg-eth1`，还有一个用于`loopback`网卡`ifcfg-lo`。操作系统在引导时会读取配置文件并配置网络接口；

## 一、网卡配置案例

### 1.1 the Dynamic Host Configuration Protocol (DHCP)
```bash
DEVICE="eth0"
NM_CONTROLLED="yes"
ONBOOT=yes
USERCTL=no
TYPE=Ethernet
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=no
NAME="System eth0"
UUID=5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03
HWADDR=08:00:27:16:C3:33
PEERDNS=yes
PEERROUTES=yes
```
### 1.2 static
```bash
DEVICE="eth0"
NM_CONTROLLED="yes"
ONBOOT=yes
USERCTL=no
TYPE=Ethernet
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=no
NAME="System eth0"
UUID=5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03
HWADDR=08:00:27:16:C3:33
IPADDR=192.168.1.101
NETMASK=255.255.255.0
BROADCAST=192.168.1.255
PEERDNS=yes
PEERROUTES=yes
```
## 二、配置参数

- **`BOOTPROTO`**: 获取IP地址的方式（How the interface obtains its IP address:）
    - `bootp`: Bootstrap Protocol (BOOTP).
    - `dhcp`: 动态获取；Dynamic Host Configuration Protocol (DHCP).
    - `static`: 静态IP
    - `none`：手动
- `BROADCAST`：IPv4广播地址；IPv4 broadcast address.
- `DEFROUTE`
Whether this interface is the default route.

- **`DEVICE`：** 接口名（设备，网卡）；Name of the physical network interface device (or a PPP logical device).

- **`HWADDR`：** 硬件地址（即MAC地址）
Media access control (MAC) address of an Ethernet device.

- **`IPADDR`**： 地址；
IPv4 address of the interface.

- `IPV4_FAILURE_FATAL`：
Whether the device is disabled if IPv4 configuration fails.

- `IPV6_FAILURE_FATAL`：
Whether the device is disabled if IPv6 configuration fails.

- `IPV6ADDR`：
IPv6 address of the interface in CIDR notation. For example: IPV6ADDR="2001:db8:1e11:115b::1/32"

- `IPV6INIT`：
Whether to enable IPv6 for the interface.

- `MASTER`:
Specifies the name of the master bonded interface, of which this interface is slave.

- `NAME`:
Name of the interface as displayed in the Network Connections GUI.

- **`NETMASK`:** 网络掩码；
IPv4 network mask of the interface.

- `NETWORK`:
IPV4 address of the network.

- `NM_CONTROLLED`:
Whether the network interface device is controlled by the network management daemon, NetworkManager.

- **`ONBOOT`**: `yes|no` 系统启动时网络接口是否有效（value：yes/no）
Whether the interface is activated at boot time.

- `PEERDNS`:
Whether the /etc/resolv.conf file used for DNS resolution contains information obtained from the DHCP server.

- `PEERROUTES`:
Whether the information for the routing table entry that defines the default gateway for the interface is obtained from the DHCP server.

- `SLAVE`:
Specifies that this interface is a component of a bonded interface.

- **`TYPE`**: 网络类型（通常为Ethernet：以太网）;
Interface type.

- `USERCTL`:
Whether users other than root can control the state of this interface.

- `UUID`:
Universally unique identifier for the network interface device.
IPv4 broadcast address.

- **`GATEWAY:`** 默认网关地址

# 参考资料

* [About Network Interfaces](https://docs.oracle.com/cd/E52668_01/E54669/html/ol7-about-netconf.html)