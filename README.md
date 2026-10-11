# Network Discovery

A local Flask and Socket.IO dashboard for discovering devices on a network and
visualizing scan results in real time.

## Features

- Discovers devices with Nmap when available, with a parallel ping fallback.
- Infers device types from hostnames, addresses, and hardware vendor prefixes.
- Displays devices in an interactive Cytoscape graph and a device list.
- Supports manual scans, recurring scans, and custom network targets.
- Exposes device history and JSON/CSV exports.
- Optionally reports basic bandwidth data from devices that support SNMP.

Only scan networks you own or are authorized to assess. Scanning results depend
on local network configuration, host firewalls, and operating-system privileges.
The built-in fallback assumes an IPv4 `/24` network.

## Requirements

- Python 3.9 or newer.
- Optional: Nmap installed on the system for enhanced scanning.

Install dependencies in a virtual environment:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

On macOS or Linux, activate the environment with
`source .venv/bin/activate` instead.

## Run

```powershell
python app.py
```

Open <http://127.0.0.1:5000>. The development server listens on localhost and
debug mode is disabled by default.

Optional configuration is read from environment variables:

| Variable | Purpose |
| --- | --- |
| `FLASK_SECRET_KEY` | Persistent Flask secret; if omitted, a random key is generated for this run. |
| `FLASK_DEBUG` | Set to `1` to enable Flask debug mode for local development. |
| `PIHOLE_URL` | Pi-hole API URL to enable the optional blacklist endpoints. |
| `PIHOLE_TOKEN` | Optional Pi-hole API token. |
| `SNMP_COMMUNITY` | SNMP community string; defaults to `public`. |

Do not commit real credentials or `.env` files.

## HTTP API

- `GET /api/devices` — current discovered devices and timestamp.
- `GET /api/history` — device history.
- `GET /api/export.json` — JSON export of devices and history.
- `GET /api/export.csv` — CSV export of devices.
- `POST /api/scan` — start a scan; optionally pass a `targets` CIDR string or list
  in the JSON body.

The dashboard also uses Socket.IO for scan controls and live updates.


