# Don't Starve Together

Fight, farm, build and explore together in the standalone multiplayer expansion to the uncompromising wilderness survival game Don't Starve.

Native Calagopus egg for the Don't Starve Together dedicated server (Steam app `343050`), installed anonymously through SteamCMD. It runs the 64-bit Linux build on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- One cluster per server: the overworld (Master shard) and, optionally, the Caves shard in the same container.
- Name, description, password, max players, game mode, PvP and ports are set from the panel on every start. Everything else stays editable in the config files.
- The console sends Lua commands to the Master shard, e.g. `c_save()`, `c_announce("Restart in 5 minutes")`, `c_listallplayers()`, `c_rollback(1)`, `c_regenerateworld()`.
- Stopping the server sends `c_shutdown(true)`: both shards save and stop before the container exits.
- Optional Workshop mods by id.
- Failed installations are reported to Calagopus as failed, with the reason.

## Cluster Token

The server needs a Klei server token and does not start without it. It then logs `E_INVALID_TOKEN` and `Your Server Will Not Start`, and never reaches the ready state.

Get one on the [Klei account page](https://accounts.klei.com/account/game/servers?game=DontStarveTogether): Add New Server, then copy the token shown under Configure Server (a long string starting with `pds-g^`). Alternatively open the console in a logged-in game client (`~`), run `TheNet:GenerateClusterToken()` and copy the contents of the `cluster_token.txt` it writes to the client's settings directory.

Paste the token into the Cluster Token variable. It is written to `DoNotStarveTogether/Cluster_1/cluster_token.txt` on every start. One token can be used by one cluster at a time.

## Server Ports

| Port   | Default | Protocol | Required |
|--------|---------|----------|----------|
| Game (Master) | 10999 | UDP | yes (primary allocation) |
| Caves  | 11000   | UDP      | yes when Caves is on |

The Shard port (10888) links the two shards and only listens on 127.0.0.1. The Steam master server and authentication ports (27016/8766 for the Master, 27017/8767 for the Caves shard) are used by the shards' Steam game server; players find the server through the Klei lobby and connect to the game ports, so these need no allocation.

## Configuration

The cluster lives in `DoNotStarveTogether/Cluster_1/`:

- `cluster.ini`: cluster-wide settings. Created on install; the panel variables and the shard link keys are written into it on every start. Other keys (e.g. `pause_when_empty`, `cluster_intention`, `vote_enabled`, `tick_rate`) can be edited and are kept.
- `Master/server.ini`, `Caves/server.ini`: per-shard ports and role, managed by the panel.
- `Master/worldgenoverride.lua` / `leveldataoverride.lua`, `Caves/worldgenoverride.lua`: world generation settings. The install creates the Caves one with the `DST_CAVE` preset; without it the Caves shard would generate a second overworld. World generation settings only apply to a new world (`c_regenerateworld()` or delete the shard's `save/` folder).
- `adminlist.txt`, `blocklist.txt`, `whitelist.txt`: one Klei user id (`KU_...`) per line.

A cluster made on a game client can be uploaded instead: stop the server, replace the contents of `Cluster_1/` with it and keep the panel variables in line with it.

## Caves

With Caves on, the Caves shard starts in the background and connects to the Master over 127.0.0.1. Its log is not shown on the console; it is in `DoNotStarveTogether/Cluster_1/Caves/server_log.txt`. Console commands run on the Master shard; `c_save()` saves both shards. When the Master stops (`c_shutdown(true)`, or a crash), the Caves shard is sent `c_shutdown(true)` as well and the container exits once both have stopped.

## Mods

Workshop Mods takes a comma-separated list of Workshop ids. On every start they are written to `mods/dedicated_server_mods_setup.lua`, which makes the server download them. Each shard also needs them enabled in its `modoverrides.lua`; if a shard has none, one is created that enables the listed mods with default settings. Edit that file to change mod options or to enable mods added later.

With Workshop Mods empty, `mods/dedicated_server_mods_setup.lua` is left as it is and can be managed by hand. It belongs to the Steam depot, so validation resets it.

## Saves

Each shard saves to `DoNotStarveTogether/Cluster_1/<shard>/save/`, with a copy of the previous session in `backup/`. The server saves at the start of every in-game day and on `c_save()` / `c_shutdown(true)`. Server logs are in `server_log.txt` next to each shard's save.

## Requirements

About 1.2 GB of memory per shard after world generation, so about 2.5 GB with Caves, growing with players and mods. The installation takes about 4.5 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the cluster (config and saves):

```
# Ignore all
*
# Except the cluster
!DoNotStarveTogether/
```
