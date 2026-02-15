# Obsidian Multi-Sync

Run multiple [Obsidian](https://obsidian.md/) instances with separate Sync accounts using Docker and an nginx reverse proxy.

## Setup

Each Obsidian instance runs in its own [linuxserver/obsidian](https://docs.linuxserver.io/images/docker-obsidian/) container with isolated config and vault storage. An nginx reverse proxy provides URL-based routing so everything runs on a single port.

| Account  | URL                          |
|----------|------------------------------|
| Personal | `http://localhost/personal/` |
| Work     | `http://localhost/work/`     |

## Usage

```bash
docker compose up -d
```

1. Open the URL for the account you want to configure
2. Open or create a vault
3. Go to **Settings > Core plugins > Sync** and enable it
4. Log into your Obsidian account and connect to your remote vault

Repeat for each account.

## Adding More Accounts

1. Add a new service in `docker-compose.yml` following the existing pattern — set `SUBFOLDER` to your desired path (e.g., `/school/`)
2. Add a matching `location` block in `nginx.conf`
3. Add the new service to the nginx `depends_on` list
4. Create the directory and `.gitkeep`: `mkdir <name> && touch <name>/.gitkeep`
5. Restart: `docker compose up -d`

## Configuration

- **PUID/PGID** — set to match your user (`id -u` / `id -g`)
- **TZ** — set to your timezone (e.g., `America/New_York`)
- Port 80 is used by default — change the nginx port mapping in `docker-compose.yml` if needed
