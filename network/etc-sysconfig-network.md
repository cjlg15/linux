# `/etc/sysconfig/network`

The `/etc/sysconfig/network` file specifies additional information that is valid to all network interfaces on the system. The following entries from `/etc/sysconfig/network` define that IPv4 networking is enabled, IPv6 networking is not enabled, the host name of the system, and the IP address of the default network gateway:

```
NETWORKING=yes
NETWORKING_IPV6=no
HOSTNAME=host20.mydomain.com
GATEWAY=192.168.1.1
```