#                    socket

###### 创建一个tcp socket （tcp套接字）

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# ...这是使用套接的功能（省略）...

# 不用的时候，关闭套接字

s.close()
```



###### 创建一个udp socket （udp套接字）

```python
import socket

# 创建udp的套接字
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# ...这里是使用套接字功能（省略）...

# 不用的时候，关闭套接字
s.close()
```

###### 说明

- 套接字使用流程与文件的使用流程很类似

1. 创建套接字
2. 使用套接字收/发数据
3. 关闭套接字

 

###### udp发送数据

```python
# -*- coding:utf-8 -*-
from socket import *

# 1. 创建udp套接字
upd_cocket = socket(AF_INET, SOCK_DGRAM)

# 2. 准备收接方的地址
# '199.168.1.103' 表示目的ip地址
# 8080 表示目的端口
dest_addr = ('199.168.1.103:8080')	# 注意 是元组 ，ip是字符串，端口号是数字

# 3. 从键盘获取数据
send_data = input("请输入数据")

# 4. 发送数据到指定的电脑上的指定程序中
udp_socket.sendto(send_data.encode('utf-8'), dest_addr)

# 5. 关闭套接字
udp_socket.close()

```

###### udp收数据

```python
# -*- coding:utf-8 -*-
from socket import *


# 1. 创建套接字
udp_socket = socket(AF_INET, SOCK_DGRAM)

# 2. 绑定本地的相关信息，如果一个网络程序不绑定，则系统会随机分配
local_addr = (' ',7788)	# ip地址和端口号，ip一般不用写表示本机的任何一个ip
udp_socket.bind(local_addr)  # 必须绑定本地自己的ip和端口

# 3. 等待接受对方的数据
recv_data = udp_socket.recvfrom(1024)	# 表示本次接受的最大字节数

# 4. 显示接受到的数据
print(recv_data[0].decode('gbk'))  # Windows默认是gbk编码，必须是gbk编码，Ubuntu默认是utf-8编码
# (b'http://www.cmsoft.cn', ('172.33.19.150', 8080))
# 发送过来的是元组，我们想要的是元组第0个元素，第1个元素是发送方的ip和端口

# 5. 关闭套接字
udp_socket.close()
```

