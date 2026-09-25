# Hytale

Hytale blends the freedom of a sandbox with the momentum of an RPG.

Native Calagopus egg for the official Hytale server. It runs on the `ghcr.io/caloptreyx/games:hytale` image (Java 25), which downloads and updates the server on start.

## First start: two logins

Hytale's server files and players both need a Hytale account, so a new server asks you to log in twice, both through the console. Each login is a link and a code to open in a browser, signed in to your Hytale account.

1. **Download login.** On the first start the console shows a URL and an authorization code from the Hytale downloader. Open the URL, log in, and the download starts automatically. The downloader saves its credentials in `.hytale-downloader-credentials.json`, so later updates run without logging in again.
2. **Server login.** Once the server is running, type `/auth login device` in the console, open the URL shown and log in. Then run `/auth persistence Encrypted` so the login survives restarts.

Without the second login (Auth Mode `authenticated`), players cannot join.

## Capabilities

- Server update on every start (Auto Update), from the `release` or `pre-release` patchline. Nothing is downloaded when the installed version is current.
- Java may use up to 95% of the server's memory limit.
- Console commands go straight to the server. Stopping the server sends `/stop`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 5520    | UDP (QUIC) | yes (primary allocation) |

## Files

The downloaded archive provides `Server/HytaleServer.jar` and `Assets.zip`. Plugins and mods go into `mods/`. The server creates its settings and world folders on first start.

## Requirements

At least 4 GB of memory, more for more players and larger view distances.
