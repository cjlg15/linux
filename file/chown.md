# chown

`chown` 是 `change owner`的简写， 主要的功能是改变档案的拥有者。
## 命令格式
```
chown [OPTION]... [OWNER][:[GROUP]] FILE...
or: chown [OPTION]... --reference=RFILE FILE...
```
## 命令功能
Change the owner and/or group of each FILE to OWNER and/or GROUP.
With --reference, change the owner and group of each FILE to those of RFILE.
## 命令参数
* `-R, --recursive`: 常用进行递归的持续变更，即连同子目录下的所有档案、目录都变更。
> Original: operate on files and directories recursively

## Example
Owner is unchanged if missing. Group is unchanged if missing, but changed to login group if impliedadj. 含蓄的；暗指的 by a `:' following a symbolicadj. 象征的；符号的；使用符号的 OWNER. OWNER and GROUP may be numeric as well as symbolic.
* `chown root /u` :Change the owner of /u to "root".
* `chown root:staff /u:Likewiseadv`. 同样地；也 同上。, but also change its group to "staff".
* `chown -hR root /u`: Change the owner of /u and subfiles to "root".
## 命令实例
更改test.txt档案的拥有者为root:
```bash
[root@localhost wangyuchuan]# chown root test.txt 
[root@localhost wangyuchuan]# ll
total 0
-rw-rw-r--. 1 root root 0 Jan 10 19:48 test.txt
```
接下来，我们再把档案的拥有者和群组改回wangyuchuan
```bash
[root@localhost wangyuchuan]# chown wangyuchuan:wangyuchuan test.txt
[root@localhost wangyuchuan]# ll
total 0
-rw-rw-r--. 1 wangyuchuan wangyuchuan 0 Jan 10 19:48 test.txt
```
更改test.txt档案的档案拥有者为test:
```
[root@localhost wangyuchuan]# chown test test.txt 
chown: invalid user: `test'
```
从结果可以看到，用户必须是已存在系统 中的账号，也就是在`/etc/passwd` 这个档案中有纪录的用户才行。