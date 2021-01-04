# TTL(Time-To-Live)

**`TTL(Time-To-Live)`的作用是限制数据包在网络中存在的时间，防止数据包不断的在IP互联网络上循环。**

TTL是IP协议包中的一个值，TTL指定数据包被路由器丢弃之前允许通过的最大网段数量，是IP数据包在网络中可以转发的最大跳数(跃点数)，有很多原因使包在一定时间内不能被传递到目的地。例如，不正确的路由表可能导致包的无限循环。


TTL字段由数据包的发送者设置，路由器转发数据包时，至少将TTL减小1。路由器将会丢弃TTL=0的数据包，并向数据包源地址发送一个类型11的ICMP报文，表示time exceeded（TTL为0），由发送者决定是否要重发。

TTL位于IPv4包的第9个字节，是一个8 bit字段。

TTL的最大值是255，推荐值是64，windows中TTL默认值保存在注册表`HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters `下的`DefaultTTL(DWORD)`中，修改设置后重启才生效。

**ping命令结果中的TTL**：

```bash
$ ping www.baidu.com
PING www.a.shifen.com (180.149.132.151): 56 data bytes
64 bytes from 180.149.132.151: icmp_seq=0 ttl=49 time=10.136 ms
64 bytes from 180.149.132.151: icmp_seq=1 ttl=49 time=7.631 ms
64 bytes from 180.149.132.151: icmp_seq=2 ttl=49 time=6.133 ms
64 bytes from 180.149.132.151: icmp_seq=3 ttl=49 time=9.461 ms
64 bytes from 180.149.132.151: icmp_seq=4 ttl=49 time=5.172 ms
64 bytes from 180.149.132.151: icmp_seq=5 ttl=49 time=5.649 ms
64 bytes from 180.149.132.151: icmp_seq=6 ttl=49 time=11.832 ms
64 bytes from 180.149.132.151: icmp_seq=7 ttl=49 time=11.615 ms
64 bytes from 180.149.132.151: icmp_seq=8 ttl=49 time=7.929 ms
64 bytes from 180.149.132.151: icmp_seq=9 ttl=49 time=8.333 ms
64 bytes from 180.149.132.151: icmp_seq=10 ttl=49 time=6.428 ms
64 bytes from 180.149.132.151: icmp_seq=11 ttl=49 time=8.931 ms
64 bytes from 180.149.132.151: icmp_seq=12 ttl=49 time=6.284 ms
```

ping -i 1 8.8.8.8后可抓到Time-to-live exceeded的数据包，wireshark抓包使用icmp.type == 11过滤对应的ICMP包：Time-to-live exceeded (Time to live exceeded in transit)，抓包中可以根据TTL值判断数据包是否被中间设备伪造。

在域名系统 (DNS)中的TTL存活时间，用以设定域名纪录的最长缓存时间。

## ttl能做什么？

通过TTL值我们能得到什么？ 其实TTL值这个东西本身并代表不了什么，对于使用者来说，**关心的问题应该是包是否到达了目的地而不是经过了几个节点后到达。**

但是TTL值还是可以得到有意思的信息的。 每个操作系统对TTL值得定义都不同，这个值甚至可以通过修改某些系统的网络参数来修改，例如Win2000默认为128，通过注册表也可以修改。而Linux大多定义为64。不过一般来说，很少有人会去修改自己机器的这个值的，这就给了我们机会可以通过ping的回显TTL来大体判断一台机器是什么操作系统。如你看到112，可能是初始128，跳了16个节点，或者是初始160，跳了48次。 