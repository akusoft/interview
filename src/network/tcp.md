# TCP

## 出现大量 `TIME_WAIT` 或 `CLOSE_WAIT` 的原因、后果及解决办法

原因：可能是客户端或服务器端代码中未关闭 socket 导致，即缺少 `socket.close()`。
