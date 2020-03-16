# tail命令

## 语法
```
tail [OPTION]... [FILE]...
```

## 概述
默认输出文件的最后10行内容到终端屏幕。

## 命令参数

### -f
监控文件变化

```
bogon:Documents wychuan$ tail -f text.log
hello
world
wychuan

bogon:Documents wychuan$ echo '你好啊' >> text.log

bogon:Documents wychuan$ tail -f text.log
hello
world
wychuan
你好啊
```

### -n, --lines=K
输出文件最后K行内容，替代默认的10行。
也可以使用`-n +K`来表示从第K行开始输出；
```
bogon:Documents wychuan$ tail -n 2 text.log
wychuan
你好啊
```

```
bogon:Documents wychuan$ tail -n +4 text.log
你好啊
```
### --pid=PID