# /etc/group

`/etc/group`档案就是在记录 GID 与组名的对应，结构如下：

```bash
[root@centos-chuan ~]# cat /etc/group | head -n 5
root:x:0:root
bin:x:1:bin,daemon
daemon:x:2:bin,daemon
sys:x:3:bin,adm
adm:x:4:adm,daemon
```

这个档案每一行代表一个群组，也是以冒号『:』作为字段的分隔符，共分为四栏，每一字段的意义是：
1. 组名：
群组名称
2. 群组密码：
通常不需要设定，这个设定通常是给『群组管理员』使用的，目前很少有机会设定群组管理 员啦！ 同样的，密码已经移动到 /etc/gshadow 去，因此这个字段只会存在一个『x』而已；
3. GID：
就是群组的 ID 啊。我们 `/etc/passwd` 第四个字段使用的GID 对应的群组名，就是由这里对应出 来的
4. 此群组的账号名称：
我们知道一个账号可以加入多个群组，那某个账号想要加入此群组时，将该账号填入这个字段即 可。 举例来说，如果我想要让 test 也加入 root 这个群组，那么在第一行的最后面加上 ,test，注意不要有空格 使成为`root:x:0:root,test`就可以

至于在 `/etc/group` 比较重要的特色在于第四栏，这里有个问题就是：『假如我同时加入多个群组， 那么我在作业的时候，到底是以那个群组为准？』 这里涉及到一个『有效群组』的概念。

##### 初始群组(initial group)
我们创建了一个新的用户test，查看此用户的一些基本信息如下：

```bash
[root@centos-chuan ~]# grep test /etc/passwd /etc/group /etc/gshadow
/etc/passwd:test:x:500:500::/home/test:/bin/bash
/etc/group:users:x:100:test
/etc/group:test:x:500:
/etc/gshadow:users:::test
/etc/gshadow:test:!::
```

如上，test在`/etc/passwd`中对应的gid是500，此即为test用户的初始群组(initial group)。

此外，在`/etc/group`档案中的users群组第四栏发现有test账号，所以users也是账号 test所属的非初始群组。

**groups:查看有效(effective group)与支持群组**

当前用户如何知道自己所支持的群组，可以使用groups命令来查看：

```bash
[test@centos-chuan ~]$ groups
test users
```

第一个输出的群组则为当前账号的有效群组(effective group)

##### 权限问题
* 如果是读取、写入、执行档案时：
由于test账号支持test,users两个群组，所以这两个群组拥有的权限，test 账号都拥有。
* 如果是创建档案：
档案的群组为该账号的有效群组，即使用groups查看的有效群组。

```bash
[test@centos-chuan ~]$ touch test
[test@centos-chuan ~]$ ll
-rw-rw-r--. 1 test test 0 1月  14 19:04 test
```

##### newgrp: 有效群组的切换
可以使用 newgrp命令进行有效群组的切换，注意使用 newgrp 是有限制的，那就是你想要切换的群 组必须是你已经有支持的群组。

```bash
[test@centos-chuan ~]$ newgrp users
[test@centos-chuan ~]$ groups
users test
[test@centos-chuan ~]$ touch test2
[test@centos-chuan ~]$ ll
-rw-rw-r--. 1 test test  0 1月  14 19:04 test
-rw-r--r--. 1 test users 0 1月  14 19:08 test2
```