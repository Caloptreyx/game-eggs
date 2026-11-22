# Rust

The only aim in Rust is to survive. Everything wants you to die - the island's wildlife and other inhabitants, the environment, other survivors. Do whatever it takes to last another night.

Native Calagopus egg, based on Pterodactyl's bundled Rust egg (MIT License). It runs in the [`ghcr.io/caloptreyx/games:rust`](https://github.com/Caloptreyx/yolks) image. The dedicated server (Steam app `258550`) is installed anonymously through SteamCMD.

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

## Staging branch

`egg-rust-staging.yml` runs the same server on Facepunch's `staging` branch, the test build of the next update; players join it with the Rust Staging Branch client (a separate Steam app for game owners). Variables, ports, console and stop are the same as above, except:

- The image's own update only knows the public branch, so it is turned off; the startup command updates the staging branch itself through SteamCMD when Auto Update is on (`Success! App '258550' already up to date.` in the console).
- Vanilla only: the Oxide and Carbon builds follow the public branch and may not load on staging.

## Autowipe

`egg-rust-autowipe.yml` is the Rust egg (same variables, frameworks and ports) plus an automatic wipe. Before every start, `autowipe.sh` (written by the installer) checks whether a wipe time has passed since the last wipe and, if so, deletes the map (`server/rust/*.sav*` and `*.map`) and, with Wipe Blueprints on, the blueprints (`player.blueprints.*.db*`). The console shows what it did (`Autowipe (forced): wiping the map ...` or `no wipe due`).

- Wipe Schedule `forced`: Facepunch's monthly forced wipe, the first Thursday of the month at Wipe Hour (UTC; the update lands at 19:00 UTC, 18:00 UTC in northern summer). `weekly`: every week on Wipe Day at Wipe Hour. `off`: never.
- The wipe happens on the first start after the wipe time; add a Calagopus schedule that restarts the server shortly after it. The very first start only records the current period and does not wipe.
- `.last-wipe` holds the start of the last wiped period (Unix time); delete it to restart the count, or set it to `0` to wipe on the next start.
