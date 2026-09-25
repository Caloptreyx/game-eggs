# BeamMP

BeamMP brings multiplayer to BeamNG.drive.

Native Calagopus egg for the official BeamMP server, downloaded from the BeamMP GitHub releases (Debian 13 build). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Any release: `latest` or a tag such as `v3.9.3`.
- Name, description, map, players, cars, tags and the other panel settings are written to `ServerConfig.toml` on every start. Names may contain quotes and `#`.
- Console commands go straight to the server, e.g. `list`, `kick <name>`, `say <message>`. Stopping the server sends `exit`.
- Failed installations (for example a release that does not exist) are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 30814   | TCP and UDP | yes (primary allocation) |

## Auth Key

The server does not start without an Auth Key. Create one at [keymaster.beammp.com](https://keymaster.beammp.com/) under Keys and enter it in the Auth Key variable.

## Maps

Set Map to the level's info file, e.g. `/levels/gridmap_v2/info.json`, `/levels/west_coast_usa/info.json`, `/levels/italy/info.json`. Custom maps go into `Resources/Client/` as a zip file.

## Mods

Client mods (zip files) go into `Resources/Client/`; server plugins (Lua) into `Resources/Server/<plugin>/`. `ServerConfig.toml` and `Resources/` are kept on reinstall.
