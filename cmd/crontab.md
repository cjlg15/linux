# crontab

## 语法

```
Usage:
 crontab [options] file
 crontab [options]
 crontab -n [hostname]
```

## 描述
crond是linux下用来周期性的执行某种任务或等待处理某些事件的一个守护进程，与windows下的计划任务类似，当安装完成操作系统后，默认会安装此服务工具，并且会自动启动crond进程，crond进程每分钟会定期检查是否有要执行的任务，如果有要执行的任务，则自动执行该任务。

Linux下的任务调度分为两类，系统任务调度和用户任务调度。

### 系统任务调度：

系统周期性所要执行的工作，比如写缓存数据到硬盘、日志清理等。在/etc目录下有一个crontab文件，这个就是系统任务调度的配置文件。

```bash
[root@iz2ze1nwnt9tc3d5p84g5wz /]# cat /etc/crontab 
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```

### 用户任务调度：

用户定期要执行的工作，比如用户数据备份、定时邮件提醒等。用户可以使用 crontab 工具来定制自己的计划任务。所有用户定义的crontab 文件都被保存在 `/var/spool/cron`目录中。其文件名与用户名一致。

```bash
[root@iz2ze1nwnt9tc3d5p84g5wz /]# ll /var/spool/cron/
total 4
-rw------- 1 root root 1143 Oct 31 11:22 root
```

### 使用者权限文件

* /etc/cron.deny
该文件中所列用户不允许使用crontab命令

* /etc/cron.allow
该文件中所列用户允许使用crontab命令

* /var/spool/cron/
所有用户crontab文件存放的目录,以用户名命名

### crontab文件的格式

1. 每一行都代表一项任务
2. 每行的每个字段代表一项设置，它的格式共分为六个字段，
3. 前五段是时间设定段，
4. 第六段是要执行的命令段，格式如下：

```
minute   hour   day   month   week   command
```

* minute： 表示分钟，可以是从0到59之间的任何整数。
* hour：表示小时，可以是从0到23之间的任何整数。
* day：表示日期，可以是从1到31之间的任何整数。
* month：表示月份，可以是从1到12之间的任何整数。
* week：表示星期几，可以是从0到7之间的任何整数，这里的0或7代表星期日。
* command：要执行的命令，可以是系统命令，也可以是自己编写的脚本文件。

在以上各个字段中，还可以使用以下特殊字符：
* 星号（*）：代表所有可能的值，例如month字段如果是星号，则表示在满足其它字段的制约条件后每月都执行该命令操作。
* 逗号（,）：可以用逗号隔开的值指定一个列表范围，例如，“1,2,5,7,8,9”
* 中杠（-）：可以用整数之间的中杠表示一个整数范围，例如“2-6”表示“2,3,4,5,6”
* 正斜线（/）：可以用正斜线指定时间的间隔频率，例如“0-23/2”表示每两小时执行一次。同时正斜线可以和星号一起使用，例如*/10，如果用在minute字段，表示每十分钟执行一次。

如：
```bash
[root@iz2ze1nwnt9tc3d5p84g5wz 201711]# crontab -l
#crm的钉钉的token
#30 * * * * /usr/local/php/bin/php /data1/htdocs/crm.com/jobs/job.php Jobs_Job_Ddtalk_Token
#30 * * * * /usr/local/php/bin/php /data1/htdocs/crm.com/jobs/job.php Jobs_Job_Ddtalk_User

#一融通的微信的token
30 * * * * /usr/local/php/bin/php /data1/htdocs/zfeasyloan/jobs/job.php Jobs_Job_Wechat_Token

#金银屋的微信的token
30 * * * * /usr/local/php/bin/php /data1/htdocs/jyw/jobs/job.php Jobs_Job_Wechat_Token

#钱隆归来的token
30 * * * * /usr/local/php/bin/php /data1/htdocs/qianlongguilai/jobs/job.php Jobs_Job_Wechat_Token

#钱隆计息
30 0 * * * /usr/local/php/bin/php /data1/htdocs/qianlongguilai/jobs/job.php Jobs_Job_Account_Interest

#钱隆债权匹配
30 1 * * * /usr/local/php/bin/php /data1/htdocs/qianlongguilai/jobs/job.php Jobs_Job_Account_Debt

#钱隆关系脚本
*/5 * * * * /usr/local/php/bin/php /data1/htdocs/qianlongguilai/jobs/job.php Jobs_Job_User_Relation

#钱隆归来补回停留在中间账户的钱（发起了提现，在sina页面不完成密码认证的）
#*/10 * * * * /usr/local/php/bin/php /data1/htdocs/qianlongguilai/jobs/job.php Jobs_Job_Account_Withdraw2ransom
```

## 参数

### Options:
```

 -u <user>  define user 用来设定某个用户的crontab服务；
 -e         edit user's crontab 编辑某个用户的crontab文件内容。如果不指定用户，则表示编辑当前用户的crontab文件。
 -l         list user's crontab 显示某个用户的crontab文件内容，如果不指定用户，则表示显示当前用户的crontab文件内容。
 -r         delete user's crontab 从/var/spool/cron目录中删除某个用户的crontab文件，如果不指定用户，则默认删除当前用户的crontab文件。
 -i         prompt before deleting 在删除用户的crontab文件时给确认提示。
 -n <host>  set host in cluster to run users' crontabs
 -c         get host in cluster to run users' crontabs
 -s         selinux context
 -x <mask>  enable debugging
```

### file：
file是命令文件的名字,表示将file做为crontab的任务列表文件并载入crontab。如果在命令行中没有指定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将它们载入crontab。

## crond服务
### 安装crontab：
yum install crontabs
### 服务操作说明：
/sbin/service crond start //启动服务
/sbin/service crond stop //关闭服务
/sbin/service crond restart //重启服务
/sbin/service crond reload //重新载入配置
### 查看crontab服务状态：
service crond status
### 手动启动crontab服务：
service crond start
### 查看crontab服务是否已设置为开机启动，执行命令：
ntsysv
### 加入开机自动启动：
chkconfig –level 35 crond on

## 常用方法：
### 创建一个新的crontab文件

```bash
$ touch root
$ vim root 
0,15,30,45 * * * * /bin/echo 'date' > /dev/console
$ crontab root
$ ls /var/spool/cron/
root
```

### 列出crontab文件
```bash
$ crontab -l
crontab  -l
0,15,30,45 * * * * /bin/echo 'date' > /dev/console
```
可以使用这种方法在$HOME目录中对crontab文件做一备份：
```bash
$ crontab -l > $HOME/mycron
```
一旦不小心误删了crontab文件，可以用上一节所讲述的方法迅速恢复。

### 编辑crontab文件
```bash
$ crontab -e
```

### 删除crontab文件

```bash
$ crontab -ri
crontab: really delete root's crontab? y
$ ls /var/spool/cron/
```

## 使用实例

### 每1分钟执行一次myCommand
```
* * * * * myCommand
```
### 实例2：每小时的第3和第15分钟执行
```
3,15 * * * * myCommand
```
### 实例3：在上午8点到11点的第3和第15分钟执行
```
3,15 8-11 * * * myCommand
```
### 实例4：每隔两天的上午8点到11点的第3和第15分钟执行
```
3,15 8-11 */2  *  * myCommand
```
### 实例5：每周一上午8点到11点的第3和第15分钟执行
```
3,15 8-11 * * 1 myCommand
```
### 实例6：每晚的21:30重启smb
```
30 21 * * * /etc/init.d/smb restart
```
### 实例7：每月1、10、22日的4 : 45重启smb
```
45 4 1,10,22 * * /etc/init.d/smb restart
```
### 实例8：每周六、周日的1 : 10重启smb
```
10 1 * * 6,0 /etc/init.d/smb restart
```
### 实例9：每天18 : 00至23 : 00之间每隔30分钟重启smb
```
0,30 18-23 * * * /etc/init.d/smb restart
```
### 实例10：每星期六的晚上11 : 00 pm重启smb
```
0 23 * * 6 /etc/init.d/smb restart
```
### 实例11：每一小时重启smb
```
* */1 * * * /etc/init.d/smb restart
```
### 实例12：晚上11点到早上7点之间，每隔一小时重启smb
```
0 23-7 * * * /etc/init.d/smb restart
```

# 参考

* http://www.cnblogs.com/peida/archive/2013/01/08/2850483.html
* http://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/crontab.html