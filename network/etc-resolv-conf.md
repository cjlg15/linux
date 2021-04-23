# `/etc/resolv.conf`

The `/etc/resolv.conf` file defines how the system uses DNS to resolve host names and IP addresses. This file usually contains a line specifying the search domains and up to three lines that specify the IP addresses of DNS server. The following entries from `/etc/resolv.conf` configure two search domains and three DNS servers:

> /etc/resolv.conf文件定义了系统如何使用DNS来解析主机名和IP地址。 该文件通常包含一行指定搜索域，最多三行指定DNS服务器的IP地址。 以下来自/etc/resolv.conf的条目配置了两个搜索域和三个DNS服务器：


```
search us.mydomain.com mydomain.com
nameserver 192.168.154.3
nameserver 192.168.154.4
nameserver 10.216.106.3
```
If your system obtains its IP address from a DHCP server, it is usual for the system to configure the contents of this file with information also obtained using DHCP.
> 如果您的系统从DHCP服务器获取其IP地址，系统通常会使用也使用DHCP获取的信息来配置此文件的内容。