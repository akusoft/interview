# Docker

## Dockerfile 中 `CMD` 与 `ENTRYPOINT` 区别

`CMD` 用于指定默认的容器主进程的启动命令。在运行时可以指定新的命令来替代镜像设置中的这个默认命令。

比如，ubuntu 镜像默认的 `CMD` 是 `/bin/bash`，若直接 `docker run -it ubuntu` 的话，会直接进入 `bash`。也可以在运行时指定运行别的命令，如 `docker run -it ubuntu cat /etc/os-release`。这就是用 `cat /etc/os-release` 命令替换了默认的 `/bin/bash` 命令了，输出了系统版本信息。

`ENTRYPOINT` 和 `CMD` 一样，都是在指定容器启动程序及参数。`ENTRYPOINT` 在运行时也可以替代，不过比 `CMD` 要略显繁琐，需要通过 `docker run` 的参数 `--entrypoint` 来指定。

当指定了 `ENTRYPOINT` 后，`CMD` 的含义就发生了改变，不再是直接的运行其命令，而是将 `CMD` 的内容作为参数传给 `ENTRYPOINT` 指令，换句话说实际执行时，将变为：

```dockerfile
<ENTRYPOINT> "<CMD>"
```

`<ENTRYPOINT> "<CMD>"` 这种形式有以下好处：

- 让镜像变成像命令一样使用：将命令用 `ENTRYPOINT` 书写，将参数用 `CMD` 传递
- 应用运行前的准备工作：这些准备工作是和容器 `CMD` 无关的，无论 `CMD` 为什么，都需要事先进行一个预处理的工作。这种情况下，可以写一个脚本，然后放入 `ENTRYPOINT` 中去执行，而这个脚本会将接到的参数（也就是 `<CMD>`）作为命令，在脚本最后执行。

## Dockerfile 中 `COPY` 与 `ADD` 区别

`COPY` 将当前工作目录中的文件或目录复制到镜像内的目标位置。类似 Linux 中的 `cp`。

`ADD` 除了满足以上所述的 `COPY` 的作用外，还增加了以下功能：

- 复制压缩文件时会自动解压
- 可以跟链接直接获取网络文件

在 `COPY` 和 `ADD` 指令中选择的时候，可以遵循这样的原则，所有的文件复制均使用 `COPY` 指令，仅在需要自动解压缩的场合使用 `ADD`。

## `docker run` 和 `docker start`

- `docker run`：从指定的 image 文件生成一个正在运行的容器实例
- `docker start`：启动指定的已停止的容器实例
