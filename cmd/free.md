# free命令

## 命令描述
free命令用来显示内存的使用情况。

## 命令语法
```free [options]```

## 命令参数

### 不带参数

```
# free
              total        used        free      shared  buff/cache   available
Mem:        3882328     2637800      222256         444     1022272     1032944
Swap:             0           0           0
```

### -h, --human
人性化显示结果数字

```
# free -h
              total        used        free      shared  buff/cache   available
Mem:           3.7G        2.5G        215M        444K        998M        1.0G
Swap:            0B          0B          0B
```

### -c, --count ++count++
显示++count++次结果，使用此参数必须带上`-s`参数；

### -s, --seconds ++seconds++
表示多少秒显示一次；

下面表示每1秒显示一次，连续显示3次；

```
[root@iz2zeaehttqhrndd2g7vivz ~]# free -h -s 1 -c 3
              total        used        free      shared  buff/cache   available
Mem:           3.7G        2.5G        215M        444K        998M        1.0G
Swap:            0B          0B          0B

              total        used        free      shared  buff/cache   available
Mem:           3.7G        2.5G        215M        444K        998M        1.0G
Swap:            0B          0B          0B

              total        used        free      shared  buff/cache   available
Mem:           3.7G        2.5G        215M        444K        998M        1.0G
Swap:            0B          0B          0B
```