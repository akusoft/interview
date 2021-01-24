# Linux

## Linux 文件权限

分为 3 种：

- `r`：可读，4
- `w`：可写，2
- `x`：可执行，1

以 `ls -l` 的输出解释含义：

```sh
total 124
drwxrwxrwx 1 howie howie   512 Jul 21  2018 apue
-rw-r--r-- 1 root  root    224 Aug 16  2018 buf.c
...
```

第一行 `total 124` 显示当前目录下总共有 124 个文件，以下每行的：

- 第一列：显示文件的类型和权限
- 第二列：显示链接数
- 第三列：显示所属用户
- 第四列：显示所属用户组
- 第五列：显示文件大小
- 第六列：显示日期
- 第七列：显示文件名

## Linux 进程状态

https://zhuanlan.zhihu.com/p/344725512
