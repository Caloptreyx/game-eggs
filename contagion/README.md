# Contagion

Contagion is a cooperative zombie survival shooter on the Source engine: escape, extraction, hunted and panic game modes, in co-op against the infected or against other players.

Native Calagopus egg for the Contagion dedicated server (Steam app `293030`), installed anonymously through SteamCMD. The server only exists for Windows; it runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, password and RCON password are written to `contagion/cfg/server.cfg` on every start; the rest of the file stays yours.
- Start map, player limit and further launch parameters or convars (Extra Arguments) from the panel.
- The server's console log is streamed to the panel. Console commands are sent to the server over RCON (local only), e.g. `status`, `changelevel <map>`, `kick <name>`, `say <text>`; the RCON password is therefore required.
- The panel's stop sends `quit` over RCON and waits for the server to exit.
- Failed installations are reported to Calagopus as failed, with the reason.

Contagion keeps no world state.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON and Steam server queries) |

## Configuration

| File | Contents |
|------|----------|
| `contagion/cfg/server.cfg` | Server settings run on every map |
| `contagion/cfg/banned_user.cfg`, `contagion/cfg/banned_ip.cfg` | Bans |
| `contagion/maps/` | Maps (`.bsp`); the map names are the file names without `.bsp` |

## Requirements

About 670 MB of memory with no players; plan for 2 GB. The installation takes about 16 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the Contagion configuration
!contagion/cfg/
```
