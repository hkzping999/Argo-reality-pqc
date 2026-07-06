# Argo-PQC-Strong

PQC 强化版 Argo + Xray 一键脚本。

## 2.2.3 更新：Reality 自定义域名 + Reality VLESS PQC

本包在 `2.2.2-pqc-strong-reality-domain` 基础上，正式把 Reality 节点纳入 VLESS PQC 强化链路：

- Reality Vision / Reality gRPC 服务端 inbound 使用 VLESS `decryption=mlkem768x25519plus...600s...`。
- Reality Vision / Reality gRPC 客户端分享链接使用 VLESS `encryption=mlkem768x25519plus...1rtt...`。
- Clash / Mihomo 的 Reality Vision 与 Reality gRPC 节点均输出 `encryption` 字段。
- Strong mode 默认禁用 0-RTT，客户端第三段固定规范为 `1rtt`，服务端第三段保持 ticket 时间 `600s`。
- Reality 的连接地址仍由 `REALITY_DOMAIN` 控制，Reality 伪装站 / SNI 仍由 `TLS_SERVER` 控制。

正确分层：

```text
REALITY_DOMAIN  → 客户端实际连接地址，例如 reality.example.com
TLS_SERVER      → Reality 伪装站 / SNI，例如 addons.mozilla.org
security        → reality
encryption      → mlkem768x25519plus.native.1rtt...
decryption      → mlkem768x25519plus.native.600s...
```

预期 Reality Vision 链接格式：

```text
vless://uuid@reality.example.com:30000?encryption=mlkem768x25519plus.native.1rtt...&flow=xtls-rprx-vision&security=reality&sni=addons.mozilla.org&fp=chrome&pbk=PUBLIC_KEY&type=tcp&headerType=none#node%20reality-vision
```

## 2.2.2 保留项：Reality 自定义连接域名

- `REALITY_DOMAIN` 留空时继续使用 `SERVER_IP`。
- `REALITY_DOMAIN` 只影响客户端 Reality 节点的连接地址，也就是分享链接中 `uuid@地址:端口` 的地址。
- Reality 伪装站 / SNI 仍然使用 `TLS_SERVER`，不会被 `REALITY_DOMAIN` 覆盖。
- 支持安装阶段输入、非交互配置文件预置、`argox -d` 后期修改。
- 手动选择 Reality 时，即使使用临时 Argo Tunnel，也不会再因为没有固定 Argo 域名而被过滤掉。

DNS 使用建议：

```text
reality.example.com A    你的 VPS IPv4
reality.example.com AAAA 你的 VPS IPv6，可选
```

如果该域名托管在 Cloudflare，请使用 DNS only / 灰云，不要开启橙云代理。

## 使用

```bash
chmod +x argox.sh
bash -n argox.sh
sudo bash argox.sh
```

非交互安装：

```bash
sudo bash argox.sh -f config-pqc-strong.conf
```

查看节点：

```bash
argox -n
```

修改 Reality 连接域名：

```bash
argox -d
```
