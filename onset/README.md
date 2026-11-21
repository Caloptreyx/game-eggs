# Onset

Onset is an open-world multiplayer sandbox on Unreal Engine 4. Servers build their own game modes, such as roleplay, cops and robbers or freeroam, as Lua packages that clients download on join.

Native Calagopus egg for the Onset dedicated server (Steam app `1204170`), installed anonymously through SteamCMD.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, short name, game mode text, password, player limit, port and the master list setting are written to `server_config.json` on every start; packages, plugins, voice and pool settings stay yours.
- The server log is shown in the console (and written to `server_log.txt`). The server reads no console input.
- The panel's stop sends SIGINT, which shuts the server down.
- Failed installations are reported to Calagopus as failed, with the reason.

The server needs OpenSSL 1.1, which Debian 13 no longer ships; the installer puts Debian 11's `libssl.so.1.1` and `libcrypto.so.1.1` into `.libssl11/`. Persistent data belongs to the packages; the shipped `sandbox` package saves `world.json` in the server root.

The shipped config loads the `mariadb` plugin, which logs `Failed loading "mariadb": libmariadbclient.so.18` because that library is not available on current systems. It only matters for packages that use MariaDB; remove `"mariadb"` from `plugins` to silence it.

## Server Ports

Onset derives its query and file ports from the game port, so the server needs three consecutive allocations with the highest as the primary:

| Port  | Default | Protocol | Notes |
|-------|---------|----------|-------|
| Game  | 7777    | UDP      | primary allocation |
| Query | 7776    | UDP      | game port - 1 |
| File  | 7775    | TCP      | game port - 2; clients download the packages here |

## Requirements

About 60 MB of memory with the shipped packages and no players; plan for 1 GB. The installation takes about 600 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration, packages and saved data:

```
# Ignore all
*
# Except the config, packages and the sandbox world
!server_config.json
!packages/
!world.json
!schematics.json
```
