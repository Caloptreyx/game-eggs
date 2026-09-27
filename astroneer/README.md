# Astroneer

Explore and reshape distant worlds.

Native Calagopus egg. The dedicated server (Steam app `728470`) only exists for Windows. It is installed anonymously through SteamCMD and runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- The server writes no log or console output under Proton; the console shows `Astroneer server listening on port <port>` once the game port is open, which marks the server as started. The server reads no console input.
- Stopping the server saves the world first.
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

The config files are in `Astro/Saved/Config/WindowsServer/`: `AstroServerSettings.ini` (server settings) and `Engine.ini` (port, encryption). The first start creates them and then stops by itself, because the owner is not set in them yet; the panel settings are applied from the next start on, so start the server again after the first start.

## Saves

The world is saved in `Astro/Saved/SaveGames/` as `SAVE_1$<date>.savegame`; each save replaces the previous file. The server saves every 15 minutes (`AutoSaveGameInterval` in `AstroServerSettings.ini`, in seconds) and when it stops.

## Stopping

The panel's stop sends SIGINT. The startup command turns it into the server's own exit request (the file `Astro/Saved/EXITREQUEST/exitrequest`): the server saves and exits within a few seconds. Killing the server skips this save, and so does sending SIGINT to the server process directly.

## Requirements

About 2-4 GB of memory and 5 GB of disk. Up to 8 players.
