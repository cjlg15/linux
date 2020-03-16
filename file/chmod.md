# chmod

档案权限的改变使用是的 `chmod` 这个指令
## 命令格式

```bash
chmod [OPTION]... MODE[,MODE]... FILE...
or: chmod [OPTION]... OCTAL-MODE FILE...
or: chmod [OPTION]... --reference=RFILE FILE...
```

## 命令功能
Change the mode of each FILE to MODE.
## 命令参数
`-R, --recursive`常用进行递归的持续变更，即连同子目录下的所有档案、目录都变更。
> operate on files and directories recursively

## 权限的设定方法有

权限的设定方法有两种， 分别可以使用数字或者是符号来进行权限的变更。

### 数字类型改变档案权限
Linux档案的基本权限就有九个，分别是 `owner/group/others`三种身份各有自己的 `read/write/execute` 权限， 档案的权限字符为：`-rwxrwxrwx`， 这九个权限是三个三个一组的！其中，我们可以使用 权限的分数对照表如下：
```
r:4
w:2
x:1
```

每种身份`(owner/group/others)`各自的三个权限`(r/w/x)`分数是需要累加的，例如当权限为：` -rwxrwx---` 分数则是：

```bash
owner = rwx = 4+2+1 = 7
group = rwx = 4+2+1 = 7
others= --- = 0+0+0 = 0
```

所以等一下我们设定权限的变更时，该档案的权限数字就是` 770`

#### example
把档案test.txt的权限全部开启。

要全部开启档案权限，即需要设置成·-rwxrwxrwx·，转换成数字权限即为`777`，设置过程如下：
```bash
[root@localhost wangyuchuan]# ll
-rw-rw-r--. 1 wangyuchuan wangyuchuan 0 Jan 10 19:48 test.txt
[root@localhost wangyuchuan]# chmod 777 test.txt 
[root@localhost wangyuchuan]# ll
-rwxrwxrwx. 1 wangyuchuan wangyuchuan 0 Jan 10 19:48 test.txt
```

### 符号类型改变档案权限
从之前的介绍中我们可以发现，档案分别有`user,group,other`三种身份。 所以我们可以使用`u,g,o`来代表这三种身份的权限，此外使用`a `来代表all亦即全部的身份。 同样，读写执行的权限`read,write,execute`分别使用`r,w,x`来代替。


Each MODE is of the form `[ugoa]*([-+=]([rwxXst]*|[ugo]))+`

```bash
chmod [ugoa] [-+=] [rwxXst] FILE...
```

```bash
		u 		+ 		r
chmod	g 		- 		w 		FILE...
		o 		=		x
		a
```
#### example
更改test.txt档案的权限为`-rwxr--r--`:

```bash
[root@localhost wangyuchuan]# chmod u=rwx,g=r,o=r test.txt 
[root@localhost wangyuchuan]# ll
-rwxr--r--. 1 wangyuchuan wangyuchuan 0 Jan 10 19:48 test.txt
```

`u,g,o`三者之间用,号分隔。

如果我不知道原先的文件属性，而我只想要增加test.txt这个档案的每个人均可 写入的权限:

```bash
root@localhost wangyuchuan]# chmod a+x test.txt 
[root@localhost wangyuchuan]# ll
-rwxr-xr-x. 1 wangyuchuan wangyuchuan 0 Jan 10 19:48 test.txt
```

而如果是要将权限去掉而不更动其他已存在的权限呢？例如要拿掉全部人的可执行权限:

```bash
root@localhost wangyuchuan]# chmod a-x test.txt 
[root@localhost wangyuchuan]# ll
-rw-r--r--. 1 wangyuchuan wangyuchuan 0 Jan 10 19:48 test.txt
```