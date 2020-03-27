# ss命令

## 语法

```bash
Usage: ss [ OPTIONS ]
       ss [ OPTIONS ] [ FILTER ]
```

## 描述

ss是`Socket Statistics`的缩写。顾名思义，ss命令可以用来获取socket统计信息，它可以显示和netstat类似的内容。但ss的优势在于它能够显示更多更详细的有关TCP和连接状态的信息，而且比netstat更快速更高效。

当服务器的socket连接数量变得非常大时，无论是使用netstat命令还是直接`cat /proc/net/tcp`，执行速度都会很慢。可能你不会有切身的感受，但请相信我，当服务器维持的连接达到上万个的时候，使用netstat等于浪费 生命，而用ss才是节省时间。

天下武功唯快不破。ss快的秘诀在于，它利用到了TCP协议栈中tcp_diag。tcp_diag是一个用于分析统计的模块，可以获得Linux 内核中第一手的信息，这就确保了ss的快捷高效。当然，如果你的系统中没有tcp_diag，ss也可以正常运行，只是效率会变得稍慢。（但仍然比 netstat要快。）

## 参数

* -h, --help	帮助信息
* -V, --version	程序版本信息
* -a, --all	显示所有套接字（sockets）

* -n, --numeric	不解析服务名称
```bash
# 查看所有正在监听的tcp socket，默认会把端口号解析成服务名称，如80端口解析成了http服务
[root@iZ2zeesy3n96eqn1rgi15oZ ~]# ss -tl
State       Recv-Q Send-Q Local Address:Port                 Peer Address:Port                
LISTEN      0      128            *:http                         *:*                    
LISTEN      0      128            *:ssh                          *:*                    
LISTEN      0      128    127.0.0.1:cslistener                   *:*                    
LISTEN      0      32            :::ftp                         :::*                    
LISTEN      0      80            :::mysql                       :::*                    
# 如果不想看服务名称，则加上-n参数，会显示出相应的端口号
[root@iZ2zeesy3n96eqn1rgi15oZ ~]# ss -tln
State       Recv-Q Send-Q Local Address:Port               Peer Address:Port              
LISTEN      0      128              *:80                           *:*                  
LISTEN      0      128              *:22                           *:*                  
LISTEN      0      128      127.0.0.1:9000                         *:*                  
LISTEN      0      32              :::21                          :::*                  
LISTEN      0      80              :::3306                        :::*
```

* -r, --resolve 解析主机名

* -l, --listening	显示监听状态的套接字（sockets）
* -o, --options        显示计时器信息
* -e, --extended       显示详细的套接字（sockets）信息
* -m, --memory         显示套接字（socket）的内存使用情况
* -p, --processes	显示使用套接字（socket）的进程
* -i, --info	显示 TCP内部信息
* -s, --summary	显示套接字（socket）使用概况
* -4, --ipv4           仅显示IPv4的套接字（sockets）
* -6, --ipv6           仅显示IPv6的套接字（sockets）
* -0, --packet	        显示 PACKET 套接字（socket）
* -t, --tcp	仅显示 TCP套接字（sockets）
* -u, --udp	仅显示 UCP套接字（sockets）
* -d, --dccp	仅显示 DCCP套接字（sockets）
* -w, --raw	仅显示 RAW套接字（sockets）
* -x, --unix	仅显示 Unix套接字（sockets）
* -f, --family=FAMILY  显示 FAMILY类型的套接字（sockets），FAMILY可选，支持  unix, inet, inet6, link, netlink
* -A, --query=QUERY, --socket=QUERY
      QUERY := {all|inet|tcp|udp|raw|unix|packet|netlink}[,QUERY]
* -D, --diag=FILE     将原始TCP套接字（sockets）信息转储到文件
*  -F, --filter=FILE   从文件中都去过滤器信息
       FILTER := [ state TCP-STATE ] [ EXPRESSION ]

## 实例

### 查看端口号是否在监听 
启动一个应用后，一般我们会查一下端口来确定应用是否启动：

```bash
[root@iZ2zeesy3n96eqn1rgi15oZ ~]# ss -tlun | grep 3306
tcp    LISTEN     0      80       :::3306                 :::*

# 上述命令等同于下述使用netstat命令
[root@iZ2zeesy3n96eqn1rgi15oZ ~]# netstat -altnp | grep 3306
tcp6       0      0 :::3306                 :::*                    LISTEN      7621/mysqld
```

### 显示tcp套接字

```bash
[root@iZ2zeesy3n96eqn1rgi15oZ ~]# ss -t -a
State      Recv-Q Send-Q                  Local Address:Port                                   Peer Address:Port                
LISTEN     0      128                                 *:http                                              *:*                    
LISTEN     0      128                                 *:ssh                                               *:*                    
LISTEN     0      128                         127.0.0.1:cslistener                                        *:*                    
ESTAB      0      0                      172.17.163.138:ssh                                   61.50.104.210:57672                
ESTAB      0      96                     172.17.163.138:ssh                                   61.50.104.210:57661                
CLOSE-WAIT 321    0                      172.17.163.138:55262                               140.205.140.205:http                 
ESTAB      0      0                      172.17.163.138:40626                                  106.11.68.13:http                 
LISTEN     0      32                                 :::ftp                                              :::*                    
LISTEN     0      80                                 :::mysql                                            :::*                      
```

### 显示 Sockets 摘要
```bash
[root@iZ2zeesy3n96eqn1rgi15oZ ~]# ss -s
Total: 122 (kernel 180)
TCP:   9 (estab 3, closed 0, orphaned 0, synrecv 0, timewait 0/0), ports 0

Transport Total     IP        IPv6
*         180       -         -        
RAW       0         0         0        
UDP       8         6         2        
TCP       9         7         2        
INET      17        13        4        
FRAG      0         0         0 
```

### 列出所有打开的网络连接端口

```bash
[root@iZ2zeesy3n96eqn1rgi15oZ ~]# ss -l
State      Recv-Q Send-Q                  Local Address:Port                                   Peer Address:Port                
LISTEN     0      128                                 *:http                                              *:*                    
LISTEN     0      128                                 *:ssh                                               *:*                    
LISTEN     0      128                         127.0.0.1:cslistener                                        *:*                    
LISTEN     0      32                                 :::ftp                                              :::*                    
LISTEN     0      80                                 :::mysql                                            :::*
```

