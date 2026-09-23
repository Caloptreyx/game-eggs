# 7 Days to Die

Set in a brutally unforgiving post-apocalyptic world overrun by the undead, 7 Days to Die is an open-world game that is a unique combination of first person shooter, survival horror, tower defense, and role-playing games. It presents combat, crafting, looting, mining, exploration, and character growth, in a way that has seen a rapturous response from fans worldwide. Play the definitive zombie survival sandbox RPG that came first. Navezgane awaits!

Native Calagopus egg. The dedicated server (Steam app `294420`) is installed anonymously through SteamCMD.

## Server Ports

| Port    | Default       | Protocol | Required |
|---------|---------------|----------|----------|
| Game    | 26900         | TCP/UDP  | yes (primary allocation) |
| Game    | 26901 - 26902 | UDP      | yes, primary + 1 and + 2 |
| Telnet  | 8081          | TCP      | only when exposed with a Telnet Password |
| Web Dashboard | 8080    | TCP      | only when enabled in `serverconfig_custom.xml` |

Allocate the two ports following the primary allocation as additional allocations.

## Configuration

The panel variables (server name, passwords, world, sandbox/difficulty code, ...) are passed on the command line and override the matching entries of the config file on every start.

Every other setting lives in `serverconfig_custom.xml`, created from the shipped `serverconfig.xml` on install. Edit that copy: `serverconfig.xml` belongs to the Steam depot and is overwritten by updates and validation.

## Console

The server writes its log to `logs/latest.log`, which is streamed to the console while the server starts. Once telnet is up the console becomes a local telnet session (through `rcon` when a Telnet Password is set), so console commands (`help`, `saveworld`, `shutdown`, ...) reach the server. The server counts as started once the world is loaded (`StartGame done`). Stopping the server sends `shutdown` and waits for it to finish saving.

## Saves

World saves, generated worlds and `serveradmin.xml` are stored in `.local/share/7DaysToDie/`.

## Sample ignore file for backups

Most files can be re-downloaded by reinstalling. Saved as `.pteroignore` in the server root, this keeps backups to your config, logs, saves and generated worlds:

```
# Ignore all
*
# Except the server config
!serverconfig_custom.xml
# Except server data dir
!.local/
# Except logs
!logs/
```
