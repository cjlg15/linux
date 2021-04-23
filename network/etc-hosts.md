# `/etc/hosts`

The `/etc/hosts` file associates host names with IP addresses. It allows the system to look up (resolve) the IP address of a host given its name, or the name given the UP address. Most networks use DNS (Domain Name Service) to perform address or name resolution. Even if your network uses DNS, it is usual to include lines in this file that specify the IPv4 and IPv6 addresses of the loopback device, for example:

```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
```

The first and second column contains the IP address and host name. Additional columns contain aliases for the host name.