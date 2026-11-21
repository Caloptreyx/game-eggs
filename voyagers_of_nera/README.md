# Voyagers of Nera

Voyagers of Nera is a cooperative survival-crafting game where you must survive a magical ocean world brimming with lost spirits and deadly monsters.

Native Calagopus egg. The dedicated server (Steam app `3937860`) only exists for Windows. It is installed anonymously through SteamCMD and runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- The server log is streamed to the console. The server reads no console input.
- Server name, password, max players, autosave interval and the gameplay settings are written to the settings files on every start.
- Stopping the server waits for the next autosave, then shuts the server down cleanly.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port   | Default | Protocol | Required |
|--------|---------|----------|----------|
| Game   | 7777    | UDP      | yes (primary allocation) |
| Beacon | 7778    | UDP      | yes, the port following the primary allocation |

The server always listens on the primary port and the next one (its beacon); allocate both. Treehouse Games notes that some hosts also needed the ports open for TCP.

## Joining

Players open **Browse Dedicated Servers** in the Multiplayer menu and pick the server by its Server Name. The server also prints an invite code on every start (`HOSTING INVITE CODE: ...`). It advertises its public IP through Epic Online Services; set **Advertised IP** only if players must connect to a different address.

## Configuration

The settings files are in `BoatGame/Saved/PersistedData/CustomConfig/`:

- `CustomHostServerUserSettings.ini`: Server Name (`HostServerDisplayName`, letters and digits only, up to 30), Server Password (`HostServerPassword`, letters and digits only), Max Players (1-10), Autosave Interval (`AutosaveTimerSeconds`, 10-900).
- `CustomGameUserSettings.ini`: Gathering Rate, Enemy Damage and Player Damage multipliers (0.01-10), Disable Equipment Durability, Keep Items On Death (`DisableDropItemsOnDeath`), Disable Weather, Disable Fog of War.

Both are written from the panel variables before every start.

## Saves

The world is stored in `BoatGame/Saved/PersistedData/<world id>/`, in two alternating save slots (`Blue/` and `Green/`) plus `SaveManifest.json`, which records the last good save of each slot. The server autosaves every Autosave Interval (default 300 seconds). To move a world from a locally hosted game, follow Treehouse Games' [transfer guide](https://treehousegames.notion.site/How-to-Transfer-Local-Save-to-Dedicated-Server-271bc50b8edb80609c28c3ee5661496c).

## Stopping

The server does not save when it shuts down. The panel's stop sends SIGINT; the startup command then waits for the next autosave (at most Autosave Interval + 30 seconds) before it asks the server to exit, so a stop can take up to that long. Killing the server loses everything since the last autosave.

## Requirements

The server used about 4.4 GB of memory with a new world and no players; plan for 6 GB, more with 10 players. The installation takes about 8 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the world and settings:

```
# Ignore all
*
# Except the Voyagers of Nera world and settings
!BoatGame/Saved/PersistedData/
```
