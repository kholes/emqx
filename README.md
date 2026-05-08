# EMQX Local Setup

Guide to run EMQX with Docker Compose for local development.

## Prerequisites

- Docker and Docker Compose are installed.

## Run EMQX

From the `emqx` folder:

```bash
docker compose up -d
docker compose ps
```

View logs:

```bash
docker compose logs -f emqx1
```

Stop service:

```bash
docker compose down
```

## Endpoints and Ports

Active services from `docker-compose.yml`:

- MQTT TCP: `1883`
- MQTT over TLS: `8883`
- WebSocket: `8083`
- WebSocket Secure: `8084`
- Dashboard HTTP: `18083`

Default local host:

- MQTT broker: `localhost`
- Dashboard: `http://localhost:18083`

## MQTT Authentication

Password-based authentication is enabled using the EMQX built-in database, with bootstrap users from `authn-users.json`.

Default MQTT credentials:

- Username: `iot_device`
- Password: `secret123`

Related files:

- `docker-compose.yml`
- `authn-users.json`

## Test with MQTT Explorer

Use the following configuration:

- Host: `localhost`
- Port: `1883`
- Username: `iot_device`
- Password: `secret123`
- Subscribe topic: `#` or `demo/qos1`

## Authorization Notes (Dev Mode)

The current compose file uses permissive mode for development:

- `EMQX_AUTHORIZATION__NO_MATCH=allow`
- `EMQX_AUTHORIZATION__SOURCES=[]`

Purpose: allow wildcard subscribe (`#`) from tools like MQTT Explorer.

## Session Configuration

In the current compose setup:

- `EMQX_MQTT__SESSION_EXPIRY_INTERVAL=7d`

This means MQTT sessions can be retained for up to 7 days (depending on client behavior).

## Troubleshooting

- No traffic in MQTT Explorer:
  - make sure `docker compose ps` shows container `emqx1` is running
  - make sure you connect to `localhost:1883` (not another host)
  - make sure username/password are correct
  - click reconnect in MQTT Explorer after container restart
- Login failed:
  - check the `authn-users.json` content again
  - restart service: `docker compose up -d`
  - check auth logs: `docker compose logs emqx1`
