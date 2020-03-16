# 防火墙配置

## 防火墙配置文件

```bash
$ cat /etc/sysconfig/iptables
```

## 常用配置参数

### -dport 
指定目标TCP/IP端口，如`--dport 80`

### -sport
指定源TCP/IP端口 如 `–sport 80`

### -p tcp 
指定协议为tcp

### -p icmp 
指定协议为ICMP
### -p udp 
指定协议为UDP
### -j DROP 
拒绝
### -j ACCEPT 
允许
### -j REJECT 
拒绝并向发出消息的计算机发一个消息
### -j LOG 
在/var/log/messages中登记分组匹配的记录
### -m mac –mac 
绑定MAC地址
### -m limit –limit 1/s 1/m 
设置时间策列
### -s 10.10.0.0或10.10.0.0/16 
指定源地址或地址段
### -d 10.10.0.0或10.10.0.0/16 
指定目标地址或地址段
### -s ! 10.10.0.0 
指定源地址以外的

作者：garyond
链接：http://www.jianshu.com/p/586da7c8fd42
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## iptables服务命令

```bash
-- 启动服务
# /etc/init.d/iptables start 
# service iptables start

-- 停止服务
# /etc/init.d/iptables stop
# service iptables stop

-- 重启服务
# /etc/init.d/iptables restart
# service iptables restart

-- 保存设置
# /etc/init.d/iptables save
# service iptables save

```

## iptables 命令

### 查看iptables的配置信息

```bash
$ iptables -L -n
```

