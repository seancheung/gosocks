# gosocks

Docker hosted alpine based shadowsocks with kcptun.

## Start Server(Shadowsocks over Kcptun)

```bash
docker run --restart=always -d -p 29900:29900/udp --name gosocks -e SS_PASSWORD=mypassword seancheung/gosocks:server
```

Kcptun and Shadowsocks share the same password set by env `SS_PASSWORD`. You can set env `SS_METHOD` to override default encryption `aes-256-cfb`.

## Server Status & Logging

```bash
docker exec gosocks sv status ssserver
docker exec gosocks sv status kcptun

docker exec gosocks tail /var/log/ssserver/current
docker exec gosocks tail /var/log/kcptun/current
```

## Minimal Server(Shadowsocks only)

```bash
docker run --restart=always -d -p 8388:8388 --name gosocks -e SS_PASSWORD=mypassword seancheung/gosocks:minimal

# logging
docker logs gosocks
```

## Kcptun Client

```bash
docker run --restart=always -d -p 12948:12948 --name gosocks-client -e SS_ADDRESS=1.2.3.4:29900 -e SS_PASSWORD=mypassword seancheung/gosocks:client
```

Set `SS_ADDRESS` to _serverip:udpport_. Config Shadowsocks client's server address to `127.0.0.1`, port to `12948`, password to `SS_PASSWORD` and Encryption to `SS_METHOD`(default `aes-256-cfb`).

For **ShadowsocksX-NG**, set *plugin* to `kcptun`, *plugin options* to `key=mypassword`.

For **ShadowsocksWindows**, Download latest release of [kcptun](https://github.com/shadowsocks/kcptun/releases), extract `client_windows_amd64.exe` to the same directory of `shadowsocks.exe`, set *plugin* to `client_windows_amd64`, *plugin options* to `key=password`. For x86 architecture, use `client_windows_386.exe` instead.
