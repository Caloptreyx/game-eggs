# Factorio

Build and maintain factories: mine resources, research technologies, build infrastructure and automate production.

Native Calagopus egg for the official Factorio headless server, downloaded from factorio.com. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Any version: `latest` (stable), `experimental`, or a version such as `2.0.77`.
- The headless server includes the Space Age expansion data. Enable it through the mod settings of your save or `mods/mod-list.json`; players need Space Age to join such a save.
- A new save is created from `config/map-gen-settings.json` and `config/map-settings.json` on first start.
- Server name, password, visibility, autosave and the other panel settings are written to `config/server-settings.json` on every start.
- Stopping the server sends `/quit`, which saves the game.
- Failed installations (for example a version that does not exist) are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 34197   | UDP      | yes (primary allocation) |

## Public server list

Set Public to `true` and fill in Factorio Username and Factorio Token (from your profile on factorio.com). Without them the server stays reachable by IP.

## Files

| Path | Contents |
|------|----------|
| `saves/` | Save files |
| `mods/` | Mods and `mod-list.json` |
| `config/` | Server, map and map generation settings, admin list |

Game files are replaced on reinstall; `saves/`, `mods/` and `config/` are kept.

## Admins

Add player names to `config/server-adminlist.json`, e.g. `["alice", "bob"]`, then restart.

## Clusterio (standalone)

`egg-factorio-clusterio.yml` runs [Clusterio](https://github.com/clusterio/clusterio), the clustered Factorio server manager, as a standalone cluster: the controller (web interface) and one host (which runs Factorio server instances) in the same server. The installer adds Node.js (newest LTS, in `.node/`, with the `libatomic` library it needs), the Factorio headless server (`factorio/`, stable or the Factorio Version set) and Clusterio with `clusterioctl`. A reinstall updates all three and keeps the cluster.

- Ports: the primary allocation is the controller's web interface (HTTP, TCP). Factorio instances get UDP ports from Factorio Ports (e.g. `34197-34200`); add those as allocations.
- On every start the console prints a login token for the Admin Account (`Web interface login token for admin: ...`); paste it into the web interface. On the first start the host's pairing request is approved automatically (`Approved this host's pairing request`).
- Console lines are `clusterioctl` commands, e.g. `instance create mymap`, `instance assign mymap local`, `instance start mymap`, `instance list`.
- Stopping stops the host first (its instances save their maps), then the controller, which writes its database.
- The installation takes about 600 MB of disk (Factorio, Node.js and Clusterio).
