# df 命令

## 语法

```
df [ [ -P ] | [  -I | -M | -i | -t | -v ] ] [ -c ] [ -T { local | remote | vfstype} ] [ -F {output1 output2 output3 …} ] [ -k ] [ -m ] [ -g ] [ -s ] [FileSystem ... | File... ]
```

```df [OPTION]... [FILE]...```

## 描述

df 命令显示文件系统的总空间和可用空间信息。FileSystem 参数指定文件系统所在的设备名、文件系统所安装的目录，或者文件系统的相对路径名。File 参数指定非安装点的文件或目录。如果指定了 File 参数，df命令显示该文件或目录所在文件系统的信息。如果您未指定 FileSystem 或 File 参数，命令 df显示当前已安装的所有文件系统信息。在缺省情况下，文件系统的统计信息以 512 字节的块单元显示。

df 命令通过 statfs系统调用得到文件系统的空间统计信息。然而，如果指定了 -s 标志，那么从虚拟文件系统 (VFS) 的文件系统帮助中取得统计信息。如果您不用 -s 标志指定参数，而且帮助系统无法获取统计信息，那么采用 statfs 系统调用统计信息。在某些例外情况下，例如运行 df 命令时，文件系统正在被修改，那么 df 命令显示的统计信息可能并不正确。

## 参数

### -h
可读性友好显示；

```
# df -h
文件系统               容量  已用  可用 已用% 挂载点
/dev/vda1               40G  5.6G   32G   15% /
devtmpfs               1.9G     0  1.9G    0% /dev
tmpfs                  1.9G     0  1.9G    0% /dev/shm
tmpfs                  1.9G  436K  1.9G    1% /run
tmpfs                  1.9G     0  1.9G    0% /sys/fs/cgroup
10.15.1.2:/data/nfs    158G  8.5G  141G    6% /data/nfs
tmpfs                  380M     0  380M    0% /run/user/0
10.15.1.2:/data/image  158G  8.5G  141G    6% /data/image
```

``` -g ```
以 GB 块为单位显示统计信息。

``` -m ```
以 MB 块为单位显示统计信息。

``` -k ```
以 1024 字节块为单位显示统计信息。