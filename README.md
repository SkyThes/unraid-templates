# Unraid Templates

Unraid Community Applications templates maintained by SkyThes.

| Template | App | Description |
|---|---|---|
| [`vaultls.xml`](./vaultls.xml) | [VaulTLS](https://github.com/7ritn/VaulTLS) | Selfhostable web app for managing mTLS certificates |
| [`z2m-device-tracker.xml`](./z2m-device-tracker.xml) | [Z2M Device Tracker](https://github.com/tempus2016/z2m-device-tracker) | History, health and alerts for every Zigbee2MQTT device |

---

# VaulTLS Unraid Template

Unraid Community Applications template for [VaulTLS](https://github.com/7ritn/VaulTLS) — a selfhostable web app for managing mTLS certificates without wrestling with shell scripts and OpenSSL.

![Unraid](https://img.shields.io/badge/Unraid-Community%20Template-F15A2C?logo=unraid&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-blue)

## About VaulTLS

VaulTLS provides a centralized platform for generating, managing, and distributing TLS and SSH certificates for your home lab, with:

- 🔒 Full TLS X.509 certificate management
- 💻 SSH certificate management
- 🔐 Optional OIDC authentication
- 🚀 ACME CA support (Traefik, acme.sh, and other ACME clients)
- 📨 Email notifications for certificate expiration

See the [VaulTLS repository](https://github.com/7ritn/VaulTLS) for full details.

## Installation

### Option 1 — Community Applications (once submitted)

Search **"VaulTLS"** in the **Apps** tab of your Unraid WebGUI and click **Install**.

### Option 2 — Manual template install

1. Download [`vaultls.xml`](./vaultls.xml) from this repo.
2. Copy it to `/boot/config/plugins/dockerMan/templates-user/` on your Unraid server.
3. In the Unraid WebGUI, go to **Docker → Add Container → Template** and select **VaulTLS**.

## Configuration

| Variable | Required | Description |
|---|---|---|
| `VAULTLS_API_SECRET` | ✅ Yes | 256-bit base64 string. Generate with `openssl rand -base64 32` |
| `VAULTLS_URL` | Recommended | Public URL where VaulTLS is reachable |
| `VAULTLS_INSECURE` | Only if no reverse proxy | Set to `true` to allow non-HTTPS access |
| `VAULTLS_DB_SECRET` | Optional | Encrypts the database. **Irreversible** — leave this variable *removed* (not just blank) if you don't want encryption |
| `VAULTLS_LOG_LEVEL` | Optional | `error`, `warn`, `info`, `debug`, or `trace` |
| `VAULTLS_OIDC_*` | Optional | OIDC login configuration |
| `VAULTLS_ACME_ENABLED` | Optional | Set to `true` to use VaulTLS as an ACME CA |

> ⚠️ **Unraid quirk:** optional variables left blank are still passed to the container as empty strings. If you're not using `VAULTLS_DB_SECRET`, delete the variable row entirely rather than leaving it empty — otherwise VaulTLS will try (and fail) to use an empty encryption key.

### Volume

| Container Path | Purpose |
|---|---|
| `/app/data` | Database, CA certificate (`ca.cert`), and CRLs |

### Port

| Container Port | Purpose |
|---|---|
| `80` | Web UI (map to any host port, e.g. `5173`) |

It's recommended to run VaulTLS behind a reverse proxy for TLS termination rather than exposing it directly.

---

# Z2M Device Tracker Unraid Template

Template for [Z2M Device Tracker](https://github.com/tempus2016/z2m-device-tracker) — history, health, alerts, network map and battery tracking for every Zigbee2MQTT device. This is the standalone Docker version of the Home Assistant app, for Home Assistant Container setups (no App store).

## Installation

1. Download [`z2m-device-tracker.xml`](./z2m-device-tracker.xml) from this repo.
2. Copy it to `/boot/config/plugins/dockerMan/templates-user/` on your Unraid server.
3. In the Unraid WebGUI, go to **Docker → Add Container → Template** and select **z2m-device-tracker**.

## Configuration

| Variable | Required | Description |
|---|---|---|
| `UI_PASSWORD` | ✅ Yes | Password for the web UI, 8 characters or more. The UI stays locked until it is set |
| `MQTT_HOST` | ✅ Yes | Hostname or IP of the MQTT broker Zigbee2MQTT uses |
| `MQTT_PORT` | Optional | Broker port (default `1883`) |
| `MQTT_USER` / `MQTT_PASS` | Optional | Broker credentials, if required |
| `HA_URL` | ✅ Yes | Base URL of Home Assistant, e.g. `http://192.168.1.10:8123` |
| `HA_TOKEN` | ✅ Yes | Long-lived access token (HA profile → Security) |
| `TZ` | Optional | Time zone, e.g. `Europe/Amsterdam` |
| `MQTT_SSL` / `MQTT_SSL_VERIFY` | Optional | MQTT over TLS, and whether to verify the certificate |

### Volumes

| Container Path | Purpose |
|---|---|
| `/data` | History, settings (`options.json`) and the HACS integration's API token |
| `/homeassistant` | Optional, read-only: Home Assistant config folder, used for the first history import. Remove the row if unused |

### Port

| Container Port | Purpose |
|---|---|
| `8099` | Web UI |

The optional HACS integration needs the app URL and an API token: run `docker exec z2m-device-tracker cat /data/api_token`.

---

## License

This repository (the Unraid template files) is licensed under [MIT](./LICENSE). VaulTLS is licensed under [GPL-3.0](https://github.com/7ritn/VaulTLS/blob/main/LICENSE); Z2M Device Tracker is licensed under [MIT](https://github.com/tempus2016/z2m-device-tracker/blob/main/LICENSE).

## Support

- Template issues: open an issue in this repo
- VaulTLS application issues: [7ritn/VaulTLS issues](https://github.com/7ritn/VaulTLS/issues)
- Z2M Device Tracker application issues: [tempus2016/z2m-device-tracker issues](https://github.com/tempus2016/z2m-device-tracker/issues)
