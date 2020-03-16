# chgrp

`chgrp` 是` change group`的简写， 主要的功能是改变一个档案的所属群组。
## 命令格式
```
chgrp [OPTION]... GROUP FILE...
or: chgrp [OPTION]... --reference=RFILE FILE...
```
## 命令功能
Change the group of each FILE to GROUP.

With --reference, change the group of each FILE to that of RFILE.

## 命令参数
* `-R, --recursive`: 常用进行递归的持续变更，即连同子目录下的所有档案、目录都变更成这个群组。
> Original: operate on files and directories recursively

## Example
```bash
chgrp staff /u:Change the group of /u to "staff".
chgrp -hR staff /u:Change the group of /u and subfiles to "staff".
```
## 命令实例
更改test.txt档案的群组为root:
```bash
[root@localhost wangyuchuan]# ll
total 0
-rw-rw-r--. 1 wangyuchuan wangyuchuan 0 Jan 10 19:48 test.txt
[root@localhost wangyuchuan]# chgrp root test.txt 
[root@localhost wangyuchuan]# ll
total 0
-rw-rw-r--. 1 wangyuchuan root 0 Jan 10 19:48 test.txt
```
更改test.txt档案的群组为test:

```
[root@localhost wangyuchuan]# chgrp test test.txt 
chgrp: invalid group: `test'
```
从结果可以看到，要被改变的组名必须要在`/etc/group` 档案内存在 才行，否则就会显示错误！