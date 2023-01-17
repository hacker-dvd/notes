+ socket

    进程之间要想进行网络通信需要 socket

    socket 服务端基本步骤：

    1. 创建 socket 对象

        ```python
        import socket
        socket_server = socket.socket()
        ```

    2. 绑定 socket_server 到指定的 IP 和地址：`socket_server.bind(host, port)`

    3. 服务端开始监听端口

        ```python
        socket_server.listen(backlog)
        # backlog 为 int 整数，表示允许的连接数量，超出的会等待，可以不填，不填会自动设置一个合理值
        ```

    4. 接收客户端连接，获得连接对象

        ```python
        conn, address = socket_server.accept()
        print(f"接收到客户端连接，连接来自：{address}")
        # accept 方法是阻塞方法，如果没有连接，会卡在这一行不向下执行
        # accept 返回的是一个二元元组
        ```

    5. 客户端连接后，通过 recv 方法，接收客户端发送的信息

        ```python
        while True:
            data = conn.recv(1024).decode("UTF-8")
            if data == 'exit':
                break
            print("接收到发送来的数据：",data)
        ```

    6. 通过 conn(客户端当此连接对象)，调用 send 方法可以回复消息

    7. conn(客户端当此连接对象)和 socket_server 对象调用 close 方法，关闭连接

    示例代码：

    ```python
    import socket
    # 创建 socket 对象
    socket_server = socket.socket()
    # 绑定 ip 地址和端口
    socket_server.bind(("localhost", 8888))
    # 监听端口
    socket_server.listen(1)  # listen 方法内接受一个整数参数，表示接受的链接数量
    result: tuple = socket_server.accept()  # accept()方法是阻塞方法，如果没有客户端链接，那么就不向下执行
    conn = result[0]  # 客户端和服务端的链接对象
    addr = result[1]  # 客户端的地址信息
    # 也可以写为 conn, addr = socket_server.accept()
    # print(addr)
    while True:
        # 接收客户端信息，要使用客户端和服务端的本次链接对象，而非 socket_server 对象
        data: str = conn.recv(1024).decode("UTF-8")
        # recv 也是阻塞式方法，接受的参数是缓冲区大小，一般给 1024 即可
        # recv 方法的返回值是一个字节数组，即 bytes 对象(不是字符串)，可以通过 decode("UTF-8")将字节数组转换为字符串对象
        print(data)  # 客户端发来的信息
        # 发送回复消息
        msg = input("请输入你要和客户端回复的信息:")
        if msg == 'exit':
            break
        msg = msg.encode("UTF-8")  # encode 可将字符串编码为字节数组对象
        conn.send(msg)
    
    # 关闭链接
    conn.close()
    socket_server.close()
    ```

+ socket 客户端编程

    基本步骤：

    1. 创建 socket 对象

        ```python
        import socket
        socket_client = socket.socket()
        ```

    2. 连接到服务端：`socket_client.connect(("localhost", 8888))`

    3. 发送消息并接收返回消息

        ```python
        while True:   # 通过无限循环来确保持续的发送消息给服务器
            send_msg = input("请输入要发送的消息")
            if send_msg == 'exit':
                break
            socket_client.send(send_msg.encode("UTF-8"))
            
            recv_data = socket_client.recv(1024)  # 阻塞式方法
            print("服务端回复的信息为：", recv_data.decode("UTF-8"))
        ```

    4. 关闭连接：`socket_client.close()`

    示例代码：

    ```python
    import socket
    socket_client = socket.socket()
    socket_client.connect(("localhost", 8888))
    while True:
        send_msg = input("请输入发送给服务端的信息")
        if send_msg == 'exit':
            break
        socket_client.send(send_msg.encode("UTF-8"))
        recv_data = socket_client.recv(1024)
        print(f"服务端返回的信息是{recv_data.decode('UTF-8')}")
    
    socket_client.close()
    ```

    