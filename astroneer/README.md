# Astroneer

Explore and reshape distant worlds.

Native Calagopus egg. The dedicated server (Steam app `728470`) only exists for Windows. It is installed anonymously through SteamCMD and runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- The server log is streamed to the console. The server reads no console input.
- Port, name, password, owner, public IP and max players are written to the config files on every start.
- Failed installations are reported to Calagopus as failed, with the reason.

## Required settings

- **Public IP:** the server's public IP address. Astroneer registers it with its backend, and players connect to it.
- **Owner Name** and **Owner Steam ID:** your Steam name and 64-bit Steam ID. The server stops on start without them, and this player gets admin rights.
- **Network Encryption** stays `False`: players cannot connect to an encrypted server running through Proton.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27000   | UDP      | yes (primary allocation); any port works |

## Configuration

The config files are in `Astro/Saved/Config/WindowsServer/`: `AstroServerSettings.ini` (server settings) and `Engine.ini` (port, encryption). They are created on first start; the panel settings are applied from the next start on, so restart the server once after the first start.

## Requirements

About 2-4 GB of memory and 5 GB of disk. Up to 8 players.
