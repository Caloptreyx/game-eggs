# Nova-Life: Amboise

Nova-Life: Amboise is a French roleplay life simulation in an open city. Players take jobs, run businesses, drive and buy vehicles and houses, and take part in an in-game economy.

Native Calagopus egg for the Nova-Life dedicated server (Steam app `1665030`), installed anonymously through SteamCMD.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- List name, slots, ports, visibility, admin PIN authentication, whitelist, autosave interval, map and frame rate are written to `Servers/server/Config/server.json` on every start. The server refuses to start without that file, so the installer seeds it; the whitelist questions, tablet URL and shop settings in it stay yours.
- The Unity log is shown in the console.
- The panel's stop sends SIGINT; the server quits cleanly.
- Failed installations are reported to Calagopus as failed, with the reason.

The server runs the server folder `Servers/server/`. On its first start it creates the other configuration files there (announcer, roleplay, car dealership, driving school, shop checkpoints); players, vehicles and objects are kept in `Servers/server/life.db`.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | TCP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |
| Steam | 27016   | UDP      | yes, additional allocation set as Steam Port |

## Requirements

About 4.4 GB of memory with no players; plan for 8 GB. The installation takes about 8 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the server folder:

```
# Ignore all
*
# Except the server folder (config and database)
!Servers/
```
