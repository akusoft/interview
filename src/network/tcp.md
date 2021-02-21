# TCP

## 出现大量 `TIME_WAIT` 或 `CLOSE_WAIT` 的原因、后果及解决办法

原因：可能是客户端或服务器端代码中未关闭 socket 导致，即缺少 `socket.close()`。

## 为什么 TCP 三次握手

参考：[为什么 TCP 建立连接需要三次握手](https://draveness.me/whys-the-design-tcp-three-way-handshake/)

## 为什么 TCP 有 TIME_WAIT

参考：[为什么 TCP 协议有 TIME_WAIT 状态](https://draveness.me/whys-the-design-tcp-time-wait/)

## 粘包

参考：[为什么 TCP 协议有粘包问题](https://draveness.me/whys-the-design-tcp-message-frame/)
