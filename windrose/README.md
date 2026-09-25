# Windrose

PvE survival adventure in the Age of Piracy: fight on land and sea, solo or with friends, build, craft and explore.

Native Calagopus egg. The dedicated server (Steam app `4129620`) only exists for Windows. It is installed anonymously through SteamCMD and runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- The server log is streamed to the console. The server reads no console input.
- Name, password, max players and connection settings are written to `R5/ServerDescription.json` on every start.
- Failed installations are reported to Calagopus as failed, with the reason.

## First start

The server creates `R5/ServerDescription.json` on its first start, including a random invite code. The panel settings are applied from the next start on, so restart the server once after the first start.

## Joining

- **Invite code (default):** players enter the invite code in the game. The connection is set up through NAT punch-through, so no ports need to be open. Leave Invite Code empty to keep the generated one (see `R5/ServerDescription.json`), or set your own.
- **Direct Connection:** players join by the server's IP and port. Needs the game port reachable from outside. The invite code does not work in this mode.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 7777    | UDP and TCP | for Direct Connection (primary allocation) |

## Requirements

| Players | Memory |
|---------|--------|
| 2       | 8 GB   |
| 4       | 12 GB  |
| 10      | 16 GB  |

About 5 GB of disk plus saves. On virtual machines, set the CPU type to host/passthrough, or the server can crash with an illegal instruction error.
