# Automobilista 2

Automobilista 2 is a racing simulator from Reiza Studios on the Madness engine, with a large roster of Brazilian and international cars and tracks, dynamic weather and time of day, and online races with practice, qualifying and race sessions.

Native Calagopus egg for the Automobilista 2 dedicated server (Steam app `1338040`), installed anonymously through SteamCMD. The server only exists for Windows; it runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, password, player limit and ports are written to `server.cfg` on every start; everything else in it stays yours.
- The server output is shown in the console (cmd.exe writes it to `server_output.log`, which is streamed). The server reads no console input.
- The panel's stop sends SIGINT, which the shell forwards to the server.
- Failed installations are reported to Calagopus as failed, with the reason.

The server keeps no world state; race results and statistics are written by the bundled Lua add-ons (`lua_config/`).

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 27015   | UDP      | yes (primary allocation; `hostPort`) |
| Query | 27016   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |
| Steam | 8766    | UDP      | yes, additional allocation set as Steam Port |

## Configuration

`server.cfg` is created once from the shipped `config_sample/server_with_lists.cfg` and documents every setting: whether the server or the first player controls the session setup (`controlGameSetup`), the HTTP API (`enableHttpApi`, `httpApiPort`; needs its own allocation), whitelists and blacklists, and the Lua add-ons. Session rotation is configured in `lua_config/sms_rotate_config.json`; the shipped example rotation names tracks this version no longer knows, so the add-on disables itself until you edit it (the server logs "Invalid track id"). `UserGuide.pdf` in the server root is the full server manual.

## Requirements

About 150 MB of memory with no players; plan for 2 GB. The installation takes about 330 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the server config and add-on configuration
!server.cfg
!lua_config/
```
