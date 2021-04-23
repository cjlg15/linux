

## 返回值分析

* `HWaddr`: 硬件mac地址
* `inet addr`: 网卡的ip地址
* `Bcast`:子网
* `Mask`: 子网掩码
* eth0：就是网络卡的代号，也有 lo 这个 loopback ；
* HWaddr：就是网络卡的硬件地址，俗称的 MAC 是也；
* inet addr：IPv4 的 IP 地址，后续的 Bcast, * Mask 分别代表的是 Broadcast 与 netmask 喔！
* inet6 addr：是 IPv6 的版本的 IP ，我们没有使用，所以略过；
* MTU：就是 MTU 啊！
* RX：那一行代表的是网络由启动到目前为止的封包接收情况， packets 代表封包数、errors 代表封包发生错误的数量、 dropped 代表封包由于有问题而遭丢弃的数量等等
* TX：与 RX 相反，为网络由启动到目前为止的传送情况；
* collisions：代表封包碰撞的情况，如果发生太多次， 表示您的网络状况不太好；
* txqueuelen：代表用来传输数据的缓冲区的储存长度；
* RX bytes, TX bytes：总传送、接收的字节总量
* Interrupt, Memory：网络卡硬件的数据， IRQ 岔断与内存地址；