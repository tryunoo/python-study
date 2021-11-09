
サーバ側
```python
# s = socket.socket()

context = ssl.create_default_context(ssl.Purpose.CLIENT_AUTH)
context.load_cert_chain(certfile="ssl/server.crt", keyfile="ssl/server.key")
context.options |= ssl.OP_NO_TLSv1 | ssl.OP_NO_TLSv1_1

sslsocket = context.wrap_socket(s, server_side=True)

data = sslsocket.read()
```

クライアント側
```
context = ssl.create_default_context(ssl.Purpose.SERVER)
context.options |= ssl.OP_NO_TLSv1 | ssl.OP_NO_TLSv1_1
server_ctx.check_hostname = False
```