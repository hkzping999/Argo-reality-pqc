# Argo-PQC-Strong Hotfix 2.2.3

This hotfix upgrades the Reality-domain build to explicitly support VLESS PQC on Reality nodes.

## 2.2.3 Reality + VLESS PQC

- Reality Vision and Reality gRPC now explicitly use VLESS Encryption / PQC when `ENABLE_VLESS_PQC='y'`.
- Server inbound keeps `decryption=mlkem768x25519plus...600s...`.
- Client URI / share-link uses `encryption=mlkem768x25519plus...1rtt...`.
- Clash / Mihomo output now includes the VLESS `encryption` field for both Reality Vision and Reality gRPC.
- Strong mode still disables 0-RTT by default.

Expected client Reality URI:

```text
vless://uuid@reality.example.com:30000?encryption=mlkem768x25519plus.native.1rtt...&flow=xtls-rprx-vision&security=reality&sni=addons.mozilla.org&fp=chrome&pbk=PUBLIC_KEY&type=tcp&headerType=none#node%20reality-vision
```

Expected server Reality inbound:

```json
"security": "reality",
"settings": {
  "decryption": "mlkem768x25519plus.native.600s...."
}
```

## 2.2.2 Reality custom connection domain retained

- `REALITY_DOMAIN` controls only the Reality client connection address.
- Empty `REALITY_DOMAIN` keeps the old behavior and uses `SERVER_IP`.
- Reality `dest` / `serverNames` still use `TLS_SERVER`.
- `argox -d` can change or clear the Reality connection domain after installation.
- Manually selected Reality protocols are preserved in temporary Argo mode because Reality now has its own connection address path.

Example:

```bash
REALITY_DOMAIN='reality.example.com'
TLS_SERVER='addons.mozilla.org'
ENABLE_VLESS_PQC='y'
VLESS_PQC_STRICT='y'
```

Deploy:

```bash
chmod +x argox.sh
bash -n argox.sh && sudo bash argox.sh -l
```

If reinstalling over a broken older install, uninstall first or remove `/etc/argox/custom` so old `vlessPqcEncryption=...600s...` does not persist.

```bash
sudo bash argox.sh -u
# or, if you want a clean reset:
sudo rm -rf /etc/argox /etc/systemd/system/xray.service /etc/systemd/system/argo.service /usr/bin/argox
sudo systemctl daemon-reload
sudo bash argox.sh -l
```
