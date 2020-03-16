# 档案隐藏权限


`SUID, SGID, SBIT` 是档案除了`r,w,x`三种权限之外的特殊权限。

使用ls查看一下`/usr/bin/passwd`档案的权限：

```bash
[root@localhost chuan]# ll -d /tmp/; ll /usr/bin/passwd 
drwxrwxrwt. 10 root root 4096 Jan 11 14:29 /tmp/
-rwsr-xr-x. 1 root root 30768 Feb 22  2012 /usr/bin/passwd
```

上述档案的权限在x的位置出现了两个特殊的权限：`s，t`

## Set UID

当 `s` 这个标志出现在档案拥有者的 `x` 权限上时，例如刚刚提到的 `/usr/bin/passwd` 这个档案的权限状 态：`-rwsr-xr-x`，此时就被称为 `Set UID`，简称为 `SUID` 的特殊权限。 那么`SUID`的权限对于一个档案的特殊功能是什么呢？基本上`SUID`有这样的限制与功能：
* `SUID` 权限仅对二进制程序(binary program)有效；
* 执行者对于该程序需要具有 `x` 的可执行权限；
* 本权限仅在执行该程序的过程中有效 (run-time)；
* 执行者将具有该程序拥有者 (owner) 的权限。

##### 举例说明
`/etc/shadow`档案记录`Linux`系统的所有账户密码，此档案的权限为

```bash
-r--------. 1 root root 959 Jan 10 19:14 /etc/shadow
```

可见此档案只有`root`可读，且只有`root`可以强制写入，其它账号都不可读写。那么问题来了，既然此档案只有`root `可以强制写入，那么其他账号是否可以修改密码呢？（新密码要写入此档案）

我们都知道，每个账号都可以通过`passwd`命令来修改自己的密码，但是这些账号明明没有`/etc/shadow `文档的权限，是如何修改密码的？这就是`SUID`权限的作用了，过程如下：

```bash
[wangyuchuan@localhost ~]$ ll /usr/bin/passwd 
-rwsr-xr-x. 1 root root 30768 Feb 22  2012 /usr/bin/passwd
```

* `wangyuchuan`账号对`/usr/bin/passwd`文档有`x`权限，表示此账号可以使用`passwd`命令，
* `/usr/bin/passwd`档案的拥有者是`root`账号，且执行权限为`s`。

* 那么，根据上述的描述，条件都已经满足（passwd为二进制、wangyuchuan对该程序具有x权限）， 前两条满足后，根据后两条描述：执行者(wangyuchuan)在执行该程序(`/usr/bin/passwd`)的过程中，将拥有该程序拥有者(root) 的权限。

* 所以wangyuchuan可以通过root来更改/etc/shadow档案的密码信息。
> 注意事项
> * `SUID` 仅可用 `binary program` 上， 不能够用 `shell script` 上面
> * `SUID` 对于目录也是无效的

## Set GID

当 `s` 标志在档案拥有者的 `x` 项目为 `SUID`， 那么当 `s` 出现在群组的 `x `时则称为` Set GID`,简称 `SGID` 的特殊权限。

与 `SUID` 不同的是，`SGID` 可以针对档案或目录来设定！如果是对档案来说， `SGID` 有如下的功能：

* SGID 对二进制程序有用
* 程序执行者对于该程序来说，需具备 x 的权限；
* 执行者在执行的过程中将会获得该程序群组的支持！

SGID 也能够用在目录上面， 当一个目录设定了 SGID 的权限后，他将具有如下的功能：
* 若用户对于此目录具有 r 与 x 的权限时，该用户能够进入此目录。
* 用户在此目录下的有效群组 (effective group)将会变成该目录的群组；
* 用途：若用户在此目录下具有 w 的权限(可以新建档案)，则使用者建立的新档案，该新档案的 群组与此目录的群组相同。

## Sticky Bit

这个 `Sticky Bit`, `SBIT` 目前只针对目录有效，对于档案已经没有效果了。 `SBIT` 对于目录的作用 是：
* 当用户对于此目录具有 w, x 权限，亦即具有写入的权限时；
* 当用户在该目录下建立档案或目录时，仅有自己与 root 才有权力删除该档案
换句话说：当甲这个用户于A 目录是具有群组或其他人的身份，并且拥有该目录 w 的权限， 这表示 『甲用户对该目录内任何人建立的目录或档案均可进行 "删除/更名/搬移" 等动作。』 不过，如果将 A 目录加上了 SBIT 的权限项目时， 则甲只能够针对自己建立的档案或目录进行删除/更名/移动等动作， 而无法删除他人的档案。

举例来说，我们的 /tmp 本身的权限是drwxrwxrwt， 在这样的权限内容下，任何人都可以在 /tmp 内新增、修改档案，但仅有该档案/目录建立者与 root 能够删除自己的目录或档案。这个特性也 是挺重要的

## SUID,SGID,SBIT权限设定

前面介绍过 SUID 与 SGID 的功能，那么如何配置文件案使成为具有 SUID 与 SGID 的权限呢？ 我们知道数字型态更改权限的方式为『三个数字』的 组合， 那么如果在这三个数字之前再加上一个数字的话，最前面的那个数字就代表这几个权限了！

4 为 `SUID`

2 为 `SGID`

1 为 `SBIT`

假设要将一个档案权限改为`-rwsr-xr-x`时，由于 s 在用户权力中，所以是 SUID ，因此， 在原先的 755 之前还要加上 4 ，也就是： `chmod 4755 filename` 来设定！

除了数字法之外，妳也可以透过符号法来处理喔！其中 SUID 为 u+s ，而 SGID 为 g+s ，SBIT 则是 o+t 啰！来看看如下的范例：

```bash
# 设定权限成为 -rws--x--x 的模样：
[root@www tmp]# chmod u=rwxs,go=x test; ls -l test
-rws--x--x 1 root root 0 Aug 18 23:47 test
# 承上，加上 SGID 与 SBIT 在上述的档案权限中！
[root@www tmp]# chmod g+s,o+t test; ls -l test
-rws--s--t 1 root root 0 Aug 18 23:47 test 
```