# TLS-Proxy
## 简要介绍
`tls-proxy` 可以看作为 V2Ray 的 `WebSocket + TLS + Web` 方案的 C 语言极简实现版，使用 `libevent2` 轻量级事件通知库编写。在硬件资源有限的环境中（如树莓派 3B，这也是我写 tls-proxy 的根本目的），tls-proxy 可以在比 v2ray 占用更少的 CPU 以及内存资源的情况下，提供更快的响应速度和代理速度。

同时，tls-proxy 也是专门为 [ss-tproxy](https://github.com/zfl9/ss-tproxy) 编写的，因为我只写了 linux 平台的 client 以及 server。`tls-client` 只提供 3 个代理端口：`redir`（代理 TCP）、`tproxy`（代理 UDP）、`tunnel`（代理 DNS）。目的只有一个：安全快速的透明代理。

## 如何编译
```bash
## base64-library
make

## openssl-1.1.0i
./Configure linux-x86_64 --prefix=/root/temp/openssl --openssldir=/root/temp/openssl no-ssl2 no-ssl3 no-shared
make && make install

## libevent-2.1.8
./configure --prefix=/root/temp/libevent --enable-static=yes --enable-shared=no CPPFLAGS='-I/root/temp/openssl/include' LDFLAGS='-L/root/temp/openssl/lib' LIBS='-ldl -lssl -lcrypto'
make && make install

## tls-client
gcc -I../base64/include -I../libevent/include -I../openssl/include -Os -s -ldl -lpthread -o tls-client tls-client.c ../base64/lib/libbase64.o ../libevent/lib/libevent.a ../libevent/lib/libevent_openssl.a ../openssl/lib/libssl.a ../openssl/lib/libcrypto.a

## tls-server
gcc -I../base64/include -I../libevent/include -Os -s -lpthread -o tls-server tls-server.c ../base64/lib/libbase64.o ../libevent/lib/libevent.a
```
