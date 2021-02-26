# Docker

## Dockerfile 中 `CMD` 与 `ENTRYPOINT` 区别

把可能需要变动的参数写到 `CMD` 里面。
然后你可以在 `docker run` 里指定参数，
这样 `CMD` 里的参数(这里是-c) 就会被覆盖掉而 `ENTRYPOINT` 里的不被覆盖。

## Dockerfile 中 `COPY` 与 `ADD` 区别

ADD 可以跟链接直接获取网络文件，复制压缩文件时会自动解压。
