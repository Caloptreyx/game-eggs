# Rust

The only aim in Rust is to survive. Everything wants you to die - the island's wildlife and other inhabitants, the environment, other survivors. Do whatever it takes to last another night.

Native Calagopus egg, based on Pterodactyl's bundled Rust egg (MIT License). It uses the `ghcr.io/pterodactyl/games:rust` image. The dedicated server (Steam app `258550`) is installed anonymously through SteamCMD.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Optional modding framework, downloaded and updated on every start: [Oxide (uMod)](https://umod.org/games/rust) or [Carbon](https://carbonmod.gg/).
- The console becomes a WebRCON session once the server is up, so console commands reach the server. `quit` (the panel's stop command) saves the world before exiting.
- Server name, description and images are set from the panel. Names like `*** [EU]  Server | 2x ***` are passed through unchanged; values cannot contain `"`, `$` or `` ` ``.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port    | Default | Protocol | Required |
|---------|---------|----------|----------|
| Game    | 28015   | UDP      | yes (primary allocation) |
| Query   | 27017   | UDP      | for the server list; must differ from the game port |
| RCON    | 28016   | TCP      | only for remote admin tools; the panel console uses it locally |
| Rust+   | 28082   | TCP      | for the Rust+ companion app; set Rust+ App Port to -1 to disable |

Add the ports you use as allocations and set the matching variables to them.

## Requirements

Rust needs a lot of memory: about 8 GB for a 3000 m procedural map, more for larger maps and plugins. The installation takes about 6 GB of disk, plus saves and plugins.

## Files

| Path | Contents |
|------|----------|
| `server/rust/` | Saves, maps and `cfg/` (`users.cfg`, `bans.cfg`, `serverauto.cfg`) |
| `oxide/` | Oxide plugins, configs and data |
| `carbon/` | Carbon plugins, configs and data |

## Wiping

Stop the server and delete the `.sav` and `.map` files in `server/rust/` for a map wipe; also delete `player.blueprints.*.db` for a blueprint wipe. Changing World Seed or World Size also starts a fresh map.
