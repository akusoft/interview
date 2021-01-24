# socket 编程

## TCP

服务端：

```python
import socket

server_port = 12000

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server_socket.bind(('', server_port))
server_socket.listen(1)

while True:
    connection_socket, addr = server_socket.accept()
    receive_message = connection_socket.recv(1024).decode()
    modified_message = receive_message.upper()
    connection_socket.send(modified_message.encode())
    connection_socket.close()
```

客户端：

```python
import socket

server_name = 'server_name'
server_port = 12000
send_message = 'hello'

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

client_socket.connect((server_name, server_port))
client_socket.send(send_message.encode())
receive_message = client_socket.recv(1024).decode()

print(receive_message)

client_socket.close()
```

## UDP

服务端：

```python
import socket

server_port = 12000

server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

server_socket.bind(('', server_port))

while True:
    receive_message, client_address = server_socket.recvfrom(2048)
    modified_message = receive_message.decode().upper()
    server_socket.sendto(modified_message.encode(), client_address)
```

客户端：

```python
import socket

server_name = 'server_name'
server_port = 12000
send_message = 'Hello'

client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

client_socket.sendto(send_message.encode(), (server_name, server_port))
receive_message, server_address = client_socket.recvfrom(2048)

print(receive_message.decode())

client_socket.close()
```
