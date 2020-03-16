# 档案隐藏属性

linux系统中的档案除了读写执行等属性之外，还有一些隐藏属性，隐藏属性的设置和读取分别 使用`chattr`和`lsattr`两个命令。

## chattr(配置档案隐藏属性)

chattr是`change attribute`的缩写，作用是配置档案的隐藏属性。

### 命令格式
`chattr [-RVf] [-+=AacDdeijsSu] [-v version] files...`
### 命令功能
配置档案的隐藏属性
### 命令参数
-R常用进行递归的持续变更，即连同子目录下的所有档案、目录都变更。
### 隐藏属性意义
* A(Atime)
当设定了A这个属性时，若你有存取此档案或目录时，他的访问时间atime 将不会被修改，可避免I/O较慢的机器过度地存取磁盘，这对速度较慢的计算机有帮助。
* a(Append Only)
当设定了a之后，这个档案将只能增加数据，而不能删除也不能修改数据，只有root才能设定这个属性。
* c(C：Compress)
这个属性设定之后，将会自动地将此档案压缩，在读取的时候将会自动的解压。但是在存储的时候，将会先进行压缩后再进行存储（对于大档案蛮有 用的）。系统以透明的方式压缩这个文件。从这个文件读取时，返回的是解压之后的数据；而向这个文件中写入数据时，数据首先被压缩之后才写入磁盘。
* D
检查压缩文件中的错误。
* d(No dump)
当 dump 程序被执行的时候，设定 d 属性将可使该档案(或目录)不会被 dump 备份
* e
* i(Immutable)
这个 i 可就很厉害了！他可以让一个档案 不能被删除、改名、设定连结、也无法写入或新增资料！ 如果目录具有这个属性，那么任何的进程只能修改目录之下的文件，不允许建立和删除文件。 只有 root 能设定此属性。
* j
* s(Secure Delete)
当档案设定了 s 属性时，如果这个档案被删除，他将会被完全的移除出这个 硬盘空间，所以如果误删了，就完全无法救回来了！ 让系统在删除这个文件时，使用0填充文件所在的区域。
* S(Sync)
一般档案是异步写入磁盘的，如果加上S这个属性之后，当你进行任何档案的修改，该改动会同步写入磁盘。
* u(Undelete)
与 s 相反的，当使用 u 来配置文件案时，如果该档案被删除了，则数据内容 其实还存在磁盘中，可以使用此属性来防止误删档案。当一个应用程序请求删除这个文件，系统会保留其数据块以便以后能够恢复删除这个文件。
注意：属性设定常见的是 a 与 i 的设定值，而且很多设定值必须要身为 root 才 能设定
### Example
给file2加上i属性，尝试编辑一下：
```bash
[root@localhost chuan]# chattr +i file2 
[root@localhost chuan]# cat >> file2
-bash: file2: Permission denied
```

再尝试一下删除、重命名呢？
```bash
[root@localhost chuan]# lsattr file2
----i--------e- file2
[root@localhost chuan]# rm file2
rm: remove regular file `file2'? y
rm: cannot remove `file2': Operation not permitted
[root@localhost chuan]# mv file2 file3
mv: cannot move `file2' to `file3': Operation not permitted
```
把file2的i属性取消，其它不变：
```bash
[root@localhost chuan]# chattr -i file2
[root@localhost chuan]# lsattr file2 
-------------e- file2
```

## lsattr(显示档案隐藏属性)

lsattr是list attribute的缩写，作用是显示档案的隐藏属性。
### 命令格式
`lsattr [-RVadlv] [files...]`
### 命令功能
显示档案的隐藏属性
### 命令参数
* `-R`: 连同子目录的数据也一并列出来！
* `-a`: 将隐藏文件的属性也秀出来；
* `-d`: 如果接的是目录，仅列出目录本身的属性而非目录内的文件名；