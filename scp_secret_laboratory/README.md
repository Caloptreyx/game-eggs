# SCP: Secret Laboratory

SCP: Secret Laboratory is a free multiplayer horror game set in the SCP Foundation: escape the facility as Class-D personnel or scientists, contain the breach as MTF, or hunt everyone as an SCP.

Native Calagopus egg for the SCP: Secret Laboratory dedicated server (Steam app `996560`), installed anonymously through SteamCMD and run through Northwood's LocalAdmin. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from a beta branch.
- Server name and max players are written to the server's gameplay config on every start.
- Console commands go to LocalAdmin and the game, e.g. `list`, `help`, `ban`, `kick`, `rnr` (restart next round), `forcestart`. Stopping the server sends `exit`.
- Failed installations are reported to Calagopus as failed, with the reason.
- Optional [EXILED](https://github.com/ExMod-Team/EXILED) plugin framework (EXILED Version), installed on top of the game's LabAPI.

The server only starts after the owner accepts Northwood's [EULA](https://link.scpslgame.com/eula) by setting Accept EULA to 1.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 7777    | UDP      | yes (primary allocation) |

## Public server list

A new server is joinable by IP address but not listed: the server says so on every start ("Your server won't be visible on the public server list"). To appear in the public server list it has to be verified by Northwood; set a `contact_email` in the gameplay config and follow Northwood's server verification process described on the [SCP:SL tech wiki](https://techwiki.scpslgame.com/).

## Configuration

The configs are in `.config/SCP Secret Laboratory/config/<port>/` (the game port): `config_gameplay.txt` (all gameplay and server settings; the server writes every setting with its default there on the first start), `config_remoteadmin.txt` (Remote Admin roles and permissions), and the ban, whitelist and reserved slot files. Edit them while the server is stopped; the Server Name and Max Players lines are rewritten from the panel on every start.

## Admins

Give players Remote Admin roles in `config_remoteadmin.txt`; see the [SCP:SL tech wiki](https://techwiki.scpslgame.com/) for the format.

## EXILED

Set EXILED Version to `latest` (or a tag such as `v9.15.3`) and reinstall to install EXILED; reinstall again to update it. EXILED's loader goes into `.config/SCP Secret Laboratory/LabAPI/plugins/global/`, its own plugins and settings into `.config/EXILED/` (`Plugins/` for EXILED plugins, `Configs/` for their settings). Plugins you add there are kept on reinstall. Each EXILED release supports specific game versions, and the game updates itself on start (Auto Update): after a game update, wait for the matching EXILED release and reinstall, or turn Auto Update off. Setting EXILED Version back to `none` does not remove an installed EXILED; delete `.config/EXILED/` and `.config/SCP Secret Laboratory/LabAPI/plugins/global/Exiled.Loader.dll` for that.

## Saves

SCP: Secret Laboratory keeps no world state: every round starts fresh. Only the config folder is worth backing up.

## Requirements

About 1.3 GB of memory with no players; plan for 2-3 GB. The installation takes about 0.9 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configs:

```
# Ignore all
*
# Except the SCP: Secret Laboratory and LocalAdmin configs
!.config/
```
