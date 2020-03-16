# umask(档案预设权限)

`umask` 就是指定当前用户在建立档案或目录的时候的权限默认值。

当你建立一个新的档案或目录时，他的默认权限是什么呢？档案或目录的默认权限与什么有关呢？没错，默认权限与`umask`有关，`umask `的值决定了当前用户建立新档案或目录的权限值。那么，如何得知或设定`umask`呢？如下：


```bash
[root@localhost ~]# umask
0022
[root@localhost ~]# umask -S
u=rwx,g=rx,o=rx
[root@localhost ~]# su - wangyuchuan
[wangyuchuan@localhost ~]$ umask
0002
[wangyuchuan@localhost ~]$ umask -S
u=rwx,g=rwx,o=rx
```

如上所示，查阅的方式有两种，一种可以直接输入 `umask` ，就可以看到数字型态的权限设定分数， 一种则是加入`-S(Symbolic) `这个选项，就会以符号类型的方式来显示出权限了！ 数字形态的权限设定分数一共有四位数字，第一位是特殊权限值，后三位分别是`u,g,o`的分数值。

在默认权限的属性上，目录与档案是不一样的。由于一般档案通常用于数据的记录，所以不需要拥有执行x的权限：

* 若使用者建立的是档案则预设没有可执行`(x)`权限，亦即只有 `rw` 这两个权限，也就是最 大为` 666` 分，预设权限如下：
`-rw-rw-rw-`
* 若用户建立的是目录，由于`x `与是否可以进入此目录有关，因此默认为所有权限均开放，亦 即为 `777` 分，预设权限如下：
`drwxrwxrwx`
> 要注意的是，`umask` 的分数指的是『该默认值需要减掉的权限！』

当要拿掉能写的权限，就是输入 2 分，而如果要拿掉能读的权限，也就是 4 分， 那么要拿掉读与写的权限，也就是 6 分，而要拿掉执行与写入的权限，也就是 3 分，见下面例子：

如上，我们使用`root`账号查看`umask`的值为`0022`，所以 `user` 并没有被拿掉任何权限， 不过 `group`与 `others` 的权限被拿掉了 `2` (也就是` w` 这个权限)，那么，当`root`用户：
* 建立新的档案时：`(-rw-rw-rw-)-(-----w--w-)=(-rw-r--r--)`

	```bash

	[root@localhost chuan]# touch testumask
	[root@localhost chuan]# ll
	-rw-r--r--. 1 root root    0 Jan 11 11:34 testumask
	```

* 建立新的目录时：`(drwxrwxrwx)-(d----w--w-)=(drwxr-xr-x)`

	```bash
	[root@localhost chuan]# mkdir testumaskdir
	[root@localhost chuan]# ll
	drwxr-xr-x. 2 root root 4096 Jan 11 11:35 testumaskdir
	```

## 命令格式
`umask [-p] [-S] [mode]`

## 命令功能
查看或设定预设权限。
## 命令参数
`-S` 常用以符号形式显示预设权限，默认以数字方式显示。
## Example
如果要更改`umask`的值，使用`$ umask 002`即可。

在预设的情况中， `root` 的 `umask` 会拿掉比较多的属性，`root` 的 `umask` 默认是 `022` ， 这是基于安全 的考虑，至于一般身份使用者，通常他们的`umask` 为 `002` ，亦即保留同群组的写入权力！