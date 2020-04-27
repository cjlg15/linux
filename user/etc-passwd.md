# /etc/passwd档案

`/etc/passwd`档案的构造是这样的：每一行都代表一个账号，有几行就代表有几个账号在你的系统中。 不过需要 特别留意的是，里头很多账号本来就是系统正常运作所必须要的，我们可以简称他为系统账号， 例如 `bin, daemon, adm, nobody` 等等，档案内容如下：

```bash
[root@centos-chuan ~]# cat /etc/passwd | head
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
uucp:x:10:14:uucp:/var/spool/uucp:/sbin/nologin
```

从此档案的内容可以看到，每一行是一个账号，由七个字段组成，每个字段之间使用:分隔，各个字段的含义如下所述：
1. 用户名：
	第1个字段是用户名，（如第一行中的root就是用户名），代表用户账号的字符串。用户名字符可以是大小写字母、数字、减号（不能出现在首位）、点以及下划线，其他字符不合法。虽然用户名中可以出现点， 但不建议使用，尤其是首位为点时，另外减号也不建议使用，因为容易造成混淆。
2. 密码：
	第2个字段存放的就是该账号的口令，为什么是’x’呢？早期的unix系统口令确实是存放在这里，但基于安全因素，后来就将其存放到`/etc/shadow`中了，在这里只用一个’x’代替。
3. UID
	这个数字代表用户标识号，也叫做uid。系统识别用户身份就是通过这个数字来的，0就是root，也就是说你可以修改test用户的uid为0，那么系统会认为root和test为同一个账户。 通常uid的取值范围是0~65535，0是超级用户（root）的标识号，1~499由系统保留，作为管理账号，普通用户的标识号从500开始，如果我们自定义建立一个普通用户，你会看到该账户的标识号是大于或等于500的。

	##### UID范围说明：

	* `0(系统管理员)`:	

		当 UID 是 0 时，代表这个账号是『系统管理员』！ 所以当你要让其他的账号 名称也具有 root 的权限时，将该账号的 UID 改为 0 即可。 不过，很不建议有多个账号的 UID 是 0

	* `1~499(系统账号)`:	

		保留给系统使用的 ID，其实除了 0 之外，其他的 UID 权限与特性并没有不一 样。默认 500， 以下的数字让给系统作为保留账号只是一个习惯。

		由于系统上面启动的服务希望使用较小的权限去运行，因此不希望使用 root 的身份去执行这些服务，所以我们就得要提供这些运作中程序的拥有者账号 才行。这些系统账号通常是不可登入的， 所以才会有 `/sbin/nologin `这个特殊的 shell存在。

		根据系统账号的由来，通常系统账号又约略被区分为两种：
		* 1~99：由 distributions 自行建立的系统账号；
		* 100~499：若用户有系统账号需求时，可以使用的账号 UID。
		* 500~65535 (可登入账号)	给一般使用者用的，事实上目前的 linux 核心 (2.6.x 版)已经可以支持到 4294967295 (2^32-1) 这么大的 UID 号码了！
		
4. GID
表示组标识号，也叫做gid。这个字段对应着`/etc/group` 中的一条记录，其实`/etc/group`和`/etc/passwd`基本上类似。
5. 用户信息说明栏
注释说明，该字段没有实际意义，通常记录该用户的一些属性，例如姓名、电话、地址等等。不过，当你使用finger的功能时就会显示这些信息的。
6. 家目录
当用户登录时就处在这个目录下。root的家目录是/root，普通用户的家目录则为`/home/username`，这个字段是可以自定义的， 比如你建立一个普通用户test1，要想让test1的家目录在/data目录下，只要修改`/etc/passwd`文件中test1那行中的该字段为/data即可。
7. Shell：
用户登录后要启动一个进程，用来将用户下达的指令传给内核，这就是shell。Linux的shell有很多种sh, csh, ksh, tcsh, bash等， 而Redhat/CentOS的shell就是bash。查看`/etc/passwd`文件，该字段中除了`/bin/bash`外还有`/sbin/nologin`比较多，它表示不允许该账号登录。 如果你想建立一个账号不让他登录，那么就可以把该字段改成/`sbin/nologin`，默认是`/bin/bash`。

我们知道很多程序的运作都与权限有关，而权限与 UID/GID 有关！因此各程序当然需要读取 `/etc/passwd `来了解不同账号的权限。 因此 `/etc/passwd` 的权限需设定为 `-rw-r--r--`， 也正是由于这样的情况，账号密码才会迁移到`/etc/shadow`档案中。