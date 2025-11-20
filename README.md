<div align="center">
  <img src="client/public/icon.svg" alt="MatchZy Auto Tournament" width="140" height="140">
  
  # MatchZy Auto Tournament
  
  ⚡ **Automated CS2 tournament management — one click from bracket creation to final scores**
  
  <p>Complete tournament automation for Counter-Strike 2 using the enhanced MatchZy plugin. Zero manual server configuration.</p>

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?logo=docker&logoColor=white)](docker-compose.yml)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)

**📚 <a href="https://mat.sivert.io/" target="_blank">Full Documentation</a>** • <a href="https://mat.sivert.io/getting-started/quick-start/" target="_blank">Quick Start</a> • <a href="https://mat.sivert.io/features/overview/" target="_blank">Features</a> • <a href="https://mat.sivert.io/roadmap/" target="_blank">Roadmap</a> • <a href="https://mat.sivert.io/guides/troubleshooting/" target="_blank">Troubleshooting</a>

</div>

---

## ✨ Features

🏆 **Tournament Brackets** — Single/Double Elimination, Round Robin, Swiss with auto-progression  
🧩 **Custom Bracket Viewer** — Bundled fork of `brackets-viewer.js` with enhanced theming, matchup centering, and MatchZy integration  
🗺️ **Interactive Map Veto** — FaceIT-style ban/pick system for BO1/BO3/BO5  
⚡ **Real-Time Updates** — WebSocket-powered live scores and player tracking  
🎮 **Auto Server Allocation** — Matches load automatically when servers are available  
👥 **Public Team Pages** — No-auth pages for teams to monitor matches and veto  
🎛️ **Admin Match Controls** — Pause, restore, broadcast, add players via RCON  
📊 **Player Tracking** — Live connection and ready status for all 10 players  
🎬 **Demo Management** — Automatic upload and download with streaming

<div align="center">
  <img src="docs/assets/preview/08-bracket-view.png" alt="Tournament Bracket View" width="800">
  <p><em>Double-elimination bracket with synchronized winner and loser paths plus interactive match zoom</em></p>
</div>

**👉 <a href="https://mat.sivert.io/screenshots/" target="_blank">View More Screenshots</a>**

---

## 🚀 Quick Start

### Prerequisites

- **Docker** and **Docker Compose** installed ([Install Docker](https://docs.docker.com/engine/install/))
- **CS2 Server(s)** with the [enhanced MatchZy plugin](https://github.com/sivert-io/matchzy/releases)

### Step 1: Install the Tournament Platform

**1. Create a directory and the Docker Compose file:**

```bash
mkdir matchzy-tournament
cd matchzy-tournament
```

Create `docker-compose.yml` with this content:

```yaml
services:
  postgres:
    image: postgres:16-alpine
    container_name: matchzy-postgres
    restart: unless-stopped
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=matchzy_tournament
    volumes:
      - postgres-data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U postgres']
      interval: 10s
      timeout: 5s
      retries: 5

  matchzy-tournament:
    image: sivertio/matchzy-auto-tournament:latest
    container_name: matchzy-tournament-api
    restart: unless-stopped
    depends_on:
      postgres:
        condition: service_healthy
    ports:
      - '3069:3069'
    environment:
      # This is your password to sign in to the admin panel
      - API_TOKEN=your-admin-password-here
      # This token is used by CS2 servers to authenticate webhooks (should be different from API_TOKEN)
      - SERVER_TOKEN=your-server-token-here
      - DATABASE_URL=postgresql://postgres:postgres@postgres:5432/matchzy_tournament
    volumes:
      - ./data:/app/data

volumes:
  postgres-data:
```

**2. Edit the tokens in `docker-compose.yml`:**

Open `docker-compose.yml` and replace:

- `your-admin-password-here` with a simple password you'll use to login (e.g., `mypassword123`)
- `your-server-token-here` with a different token for CS2 servers (e.g., `server-token-456`)

These don't need to be super secure—just something you can remember.

**3. Start the platform:**

```bash
docker compose up -d
```

**4. Access the dashboard:**

Open `http://localhost:3069` in your browser.

**5. Login:**

You'll see the login form in the center of the screen. Enter the password you set for `API_TOKEN` in the `docker-compose.yml` file.

That's it! The tournament platform is now running. 🎉

### Step 2: Set Up CS2 Servers

You need at least one CS2 server with the [enhanced MatchZy plugin](https://github.com/sivert-io/matchzy/releases) installed.

**Recommended: CS2 Server Manager** ⭐

The easiest way to set up CS2 servers. One command installs everything:

```bash
wget https://raw.githubusercontent.com/sivert-io/cs2-server-manager/master/install.sh
bash install.sh
```

**👉 <a href="https://mat.sivert.io/getting-started/quick-start/" target="_blank">See the complete Quick Start Guide</a>** for detailed CS2 server setup, configuration, and tournament creation.

---

## ⚙️ CS2 Server Plugin

> [!CAUTION]
> This project requires an **enhanced version of MatchZy** with additional event tracking.
>
> The official MatchZy release does not expose all the granular match and player events required for full automation.

**Download:** <a href="https://github.com/sivert-io/matchzy/releases" target="_blank">sivert-io/matchzy/releases</a>

**👉 <a href="https://mat.sivert.io/getting-started/quick-start/#cs2-server-setup" target="_blank">Complete installation guide</a>**

Requires <a href="https://docs.cssharp.dev/guides/getting-started/" target="_blank">CounterStrikeSharp</a> to be installed first.

---

## 🖥️ CS2 Server Manager

Need a quick way to spin up several CS2 servers that are pre-wired for MatchZy Auto Tournament? Check out the companion project **[CS2 Server Manager](https://github.com/sivert-io/cs2-server-manager)**.

- Deploys 3–5 dedicated servers (SteamCMD + CounterStrikeSharp) in minutes
- Installs the MatchZy enhanced fork, CounterStrikeSharp, Metamod:Source, and CS2-AutoUpdater automatically
- Ships with `manage.sh` for interactive or scripted installs, updates, and repairs
- Preserves your overrides (`overrides/game/csgo/`) across updates, including MatchZy configs

**👉 <a href="https://mat.sivert.io/guides/cs2-server-manager/" target="_blank">CS2 Server Manager Guide</a>**

---

## 🤝 Contributing

Contributions are welcome! Whether you're fixing bugs, adding features, improving docs, or sharing ideas.

**👉 <a href=".github/CONTRIBUTING.md" target="_blank">Read the Contributing Guide</a>**

---

## 📜 License

MIT License - see [LICENSE](LICENSE) for details

**Credits:** <a href="https://github.com/ghostcap-gaming/cs2-map-images" target="_blank">ghostcap-gaming/cs2-map-images</a> • <a href="https://github.com/Drarig29/brackets-manager.js" target="_blank">brackets-manager.js</a> • <a href="https://github.com/Drarig29/brackets-viewer.js" target="_blank">brackets-viewer.js</a> (customized copy vendored in `client/src/brackets-viewer`)

---

<div align="center">
  <strong>Made with ❤️ for the CS2 community</strong>
</div>
