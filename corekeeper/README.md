# Core Keeper

Explore an endless cavern of creatures, relics and resources in a mining sandbox adventure. Mine, build, fight, craft and farm to unravel the mystery of the ancient Core.

Native Calagopus egg for the Linux dedicated server (Steam app `1963720`), installed anonymously through SteamCMD. It runs on the `ghcr.io/caloptreyx/games:corekeeper` image: `steamcmd:debian` plus the virtual display (Xvfb) the server needs.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Direct connections on the allocated port, so players on every platform can join by IP (cross-play).
- The Game ID and connection details from `GameInfo.txt` are printed to the console once the server is up.
- World name, Game ID, password, max players, world slot, mode and seed are set from the panel.
- Failed installations are reported to Calagopus as failed, with the reason.

## Joining

- **Steam players:** Join Game > enter the Game ID shown in the console.
- **Other platforms:** Join Game via IP with the server's IP, port and Password.

Leave Game ID or Password empty to let the server generate them; they are shown in the console on every start.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP      | yes (primary allocation) |

## Worlds

Saves are stored in `data/worlds/`. World Index selects the save slot (0-29), so several worlds can live side by side. World Mode and World Seed only apply when a world is created.

To use custom world generation settings, create a world in the game, copy its file from the game's `worldgenparams` folder to `data/worldgenparams/`, name it after the World Index, and start a new world with that index.

## Requirements

About 1-2 GB of memory for a few players.
