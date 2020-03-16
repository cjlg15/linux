# uname命令

## 语法
``` uname [OPTION]... ```

## 功能
`uname`命令用来查看电脑和操作系统信息；
`uname`是`Unix name`的缩写；

## 参数

### -a, --all
显示所有系统信息；依次为内核名称、主机名、内核版本号、内核版本、硬件名、处理器类型、硬件平台类型、操作系统名称；

```
# uname -a
Linux iz2zeaehttqhrndd2g7vivz 3.10.0-327.22.2.el7.x86_64 #1 SMP Thu Jun 23 17:05:11 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
```

### -s 内核名称
你可以用-s参数，显示内核名称。（译注：可以在其他的类Unix系统上运行这个命令看看，比如mac就会显示Darwin）
```
# uname -s
Linux
```

### -r 内核发行版
如果你想知道你正在使用哪个内核发行版（指不同的内核打包版本），就可以用-r参数
```
# uname -r
3.10.0-327.22.2.el7.x86_64
```

### -v 内核版本
除一些内核信息外，用-v参数uname也能获取更详细的内核版本信息（译注：不是版本号，是指该内核建立的时间和CPU架构等）。
```
# uname -v
#1 SMP Thu Jun 23 17:05:11 UTC 2016
```

### -n, 节点名
显示主机在网络节点上的名称或主机名称；
```
# uname -n
iz2zeaehttqhrndd2g7vivz
```
对于RedHat和CentOS用户来说，你也可以通过/etc/redhat_release文件来查看：
```
# sudo cat /etc/redhat-release
CentOS Linux release 7.2.1511 (Core)
```

如果不是基于RedHat的发行版，你可以查看/etc/issue文件.类似如下：
### -m, --machine 硬件名称
显示主机的硬件(cpu)名；
```
# uname -m
x86_64
```
i686表明了你用的是32位的操作系统，如果是X86_64则表明你用的是64位的系统。
### -i 硬件平台
显示硬件平台类型或unknown
```
# uname -i
x86_64
```
i386意味这是正在运行一个32位的系统，如果输出的是X86_64则说明你正在运行一个64位的系统。

### -p, --processor 处理器类型
显示处理器类型或unknown
```
# uname -p
x86_64
```

### -o, --operating-system
输出操作系统名称；

```
# uname -o
GNU/Linux
```







