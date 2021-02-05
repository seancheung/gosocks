# gosocks

Docker hosted alpine based shadowsocks with kcptun.

## Start Server(Shadowsocks over Kcptun)

> [shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go)

> [kcptun](https://github.com/xtaci/kcptun)

```bash
docker run --restart=always -d -p 29900:29900/udp --name gosocks -e SS_PASSWORD=mypassword seancheung/gosocks:server
```

Kcptun and Shadowsocks share the same password set by env `SS_PASSWORD`. You can set env `SS_METHOD` to override default encryption `aes-256-cfb`.

To append other options, use env `KCP_OPTIONS` for kcptun server and `SS_OPTIONS` for shadowsocks server.

e.g.

```bash
docker run --restart=always -d -p 29900:29900/udp --name gosocks -e SS_PASSWORD=mypassword -e KCP_OPTIONS='--nocomp --mode=normal' -e SS_OPTIONS='-u' seancheung/gosocks:server
```

## Server Status & Logging

```bash
docker exec gosocks sv status ssserver
docker exec gosocks sv status kcptun

docker exec gosocks tail /var/log/ssserver/current
docker exec gosocks tail /var/log/kcptun/current
```

## Minimal Server(Shadowsocks only)

> [shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go)

```bash
docker run --restart=always -d -p 8388:8388 --name gosocks -e SS_PASSWORD=mypassword seancheung/gosocks:minimal

# logging
docker logs gosocks

# append options
docker run --restart=always -d -p 8388:8388 --name gosocks -e SS_PASSWORD=mypassword -e SS_OPTIONS='-u' seancheung/gosocks:minimal
```

## Kcptun Client

> [kcptun](https://github.com/xtaci/kcptun)

```bash
docker run --restart=always -d -p 12948:12948 --name gosocks-client -e SS_ADDRESS=1.2.3.4:29900 -e SS_PASSWORD=mypassword seancheung/gosocks:client
```

Set `SS_ADDRESS` to _serverip:udpport_. Config Shadowsocks client's server address to `127.0.0.1`, port to `12948`, password to `SS_PASSWORD` and Encryption to `SS_METHOD`(default `aes-256-cfb`).

Set `KCP_OPTIONS` to pass custom options. e.g. `-e KCP_OPTIONS='--nocomp'`.

For **ShadowsocksX-NG**, set *plugin* to `kcptun`, *plugin options* to `key=mypassword`. Use `;` to separate each option: `key=password;nocomp`.

For **ShadowsocksWindows**, Download latest release of [kcptun](https://github.com/shadowsocks/kcptun/releases), extract `client_windows_amd64.exe` to the same directory of `shadowsocks.exe`, set *plugin* to `client_windows_amd64`, *plugin options* to `key=password`. Use `;` to separate each option: `key=password;nocomp`. For x86 architecture, use `client_windows_386.exe` instead.


To use **ShadowRocket** on iOS, you'll need to add `--nocomp` options to server using  env `KCP_OPTIONS`.
> Ping Test does not work over KCPTUN.

## Shadowsocks(With UDP relay)

> [go-shadowsocks2](https://github.com/shadowsocks/go-shadowsocks2)

> default encryption method is `AEAD_CHACHA20_POLY1305`

```bash
docker run -d --name gosocks-ss2 --restart always -p 8488:8488 -p 8488:8488/udp -e SS_PASSWORD=mypassword seancheung/gosocks:ss2
```

## Shadowsocks with v2ray support

> [v2ray-plugin](https://github.com/shadowsocks/v2ray-plugin)

```bash
docker run -d --name gosocks-v2ray --restart always -p 8488:8488 -p 8488:8488/udp -e SS_PASSWORD=mypassword seancheung/gosocks:v2ray
```

Set `SS_PLUGIN_OPTS` to pass v2ray plugin options.

```bash
docker run -d --name gosocks-v2ray --restart always -p 8488:8488 -e SS_PLUGIN_OPTS='server;path=/v2ray;host=mydomian.com' -e SS_PASSWORD=mypassword seancheung/gosocks:v2ray
```

## V2ray(v2ray-core)

> [v2ray-core](https://github.com/v2fly/v2ray-core)

*/etc/v2ray.json*

```json
{
  "inbounds": [{
    "port": 8488,
    "protocol": "vmess",
    "settings": {
      "clients": [{ "id": "<client-uuid>" }]
    }
  }],
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  }]
}
```

```bash
docker run -d --name godocks-v2ray-core --restart always -p 8488:8488 -p 8488:8488/udp -v /etc/v2ray.json:/etc/v2ray.json seancheung/gosocks:v2ray-core -c /etc/v2ray.json
```