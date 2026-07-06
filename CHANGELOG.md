# Changelog

## 2.2.3-pqc-strong-reality-domain-reality-pqc - 2026-07-06

- Reality Vision / Reality gRPC explicitly join the VLESS PQC strong path.
- Reality server inbound keeps `decryption=mlkem768x25519plus...600s...`.
- Reality client VLESS URI keeps `encryption=mlkem768x25519plus...1rtt...`.
- Clash / Mihomo Reality Vision and Reality gRPC output includes the VLESS `encryption` field.
- Keeps `REALITY_DOMAIN` as the client connection address and `TLS_SERVER` as the Reality SNI/camouflage domain.
- Keeps strong mode defaults: `ENABLE_VLESS_PQC='y'`, `VLESS_PQC_STRICT='y'`, `VLESS_PQC_DISABLE_0RTT='y'`.

## 2.2.2-pqc-strong-reality-domain - 2026-07-06

- Added Reality custom connection domain via `REALITY_DOMAIN`.
- Added install-time input, non-interactive config support, and `argox -d` modification support.
- Preserved manually selected Reality protocols in temporary Argo mode.
