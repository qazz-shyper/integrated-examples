介绍：

Xray 前置（监听 443 端口），利用 VLESS+Vision+TLS 回落及分流 WebSocket 特性与 Caddy 为 H2C 与 gRPC 提供反向代理、为 forwardproxy 插件提供正向代理，实现除 Xray 的 mKCP 应用外共用 443 端口，其应用如下：

1、E=VLESS+Vision+TLS（回落/分流配置，TLS由自己启用及处理。）

2、B=VMess+WebSocket+TLS（TLS由VLESS+Vision+TLS启用及处理，不需配置。套CDN使用。）

3、D=VLESS+H2C+TLS（TLS由VLESS+Vision+TLS启用及处理，不需配置。）

4、G=Shadowsocks+gRPC+TLS（TLS由VLESS+Vision+TLS启用及处理，不需配置。推荐套CDN使用。）

5、A=VLESS+mKCP+seed

6、N=NaiveProxy（基于Caddy的改进版forwardproxy插件实现，TLS由VLESS+Vision+TLS启用及处理，不需配置。）

注意：

1、Xray 的监听地址不支持 Shadowsocks（简称SS） 协议使用 UDS 监听。

2、Xray 版本不小于 v1.7.2 才完美支持 VLESS 协议的 XTLS Vision 应用。

3、Caddy 版本不小于 v2.2.0 才支持 H2C proxy，即支持 Xray 的 H2C（gRPC） 反向代理。Caddy 版本不小于 v2.6.0 才支持 H2C proxy 的 UDS 转发。

4、Caddy 支持 HTTP/1.1 server 与 H2C server 共用一个端口或一个进程。

5、使用本人 Releases 中编译好的 Caddy 文件，可同时支持 H2C server、H2C proxy、NaiveProxy、Trojan-Go 等应用。

6、本示例中 NaiveProxy 仅支持 HTTP/2 代理应用，即 HTTPS 协议传输。

7、本示例所需 TLS 证书由 Caddy（内置 ACME 客户端） 提供，实现 TLS 证书自动申请及更新。

8、配置1：使用 Local Loopback 连接，且启用了 PROXY protocol。配置2：使用 UDS 连接（对应 Shadowsocks+gRPC+TLS 除外），且启用了 PROXY protocol。
