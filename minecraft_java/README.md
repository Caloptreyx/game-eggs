# Minecraft: Java Edition (Vanilla)

The official Minecraft: Java Edition server from Mojang, without plugins or mods.

Native Calagopus egg. The server jar is downloaded from Mojang and checked against its published checksum. It runs on the `ghcr.io/caloptreyx/java` images (Eclipse Temurin JRE, one image per Java version).

## Capabilities

- Any release or snapshot: `latest`, `snapshot`, or a version such as `1.21.4`.
- Calagopus's EULA prompt: on first start the console asks to accept the Minecraft EULA and writes `eula.txt`.
- Calagopus's Java version prompt: if the selected image is too old for the installed version, the console offers to switch images.
- Memory follows the server's limit: Java may use up to 95% of it.
- Stopping the server sends `stop`, which saves the world.
- Failed installations (for example a version that does not exist) are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 25565   | TCP      | yes (primary allocation) |
| Query | 25565   | UDP      | same port as the game; used by server list sites |

The panel writes the allocation's port into `server.properties` on every start.

## Java version

Select the image in the server's Startup settings. The installer prints the Java version the installed Minecraft version needs, taken from Mojang's version metadata:

| Minecraft | Java image |
|-----------|------------|
| up to 1.16.5 | Java 8 |
| 1.17 - 1.20.4 | Java 17 (1.17.x officially needs 16) |
| 1.20.5 - 1.21.x | Java 21 |
| 26.1 and newer | Java 25 |

## Changing versions

Change Minecraft Version and reinstall. Worlds and settings are kept; only the jar is replaced. Back up first: a world opened by a newer version cannot be opened by an older one again.

## Requirements

About 2 GB of memory for a few players; more for large view distances, many players or big worlds.

## Paper

`egg-minecraft-java-paper.yml` runs [Paper](https://papermc.io/), the high-performance server that loads Bukkit, Spigot and Paper plugins (put them in `plugins/`). It is downloaded from PaperMC's download service (Fill) and checked against the published SHA-256.

- Minecraft Version: `latest` picks the newest version that has a stable Paper build; or a version such as `1.21.10`.
- Build Number: `latest` for the newest stable build (the newest build of any channel when a version has no stable one yet), or a build number.
- The installer prints the Java version Paper requires for the installed version (for example 25 for 26.x, 21 for 1.21.x); select that image.
- The installer creates `server.properties` with the port keys, so the panel's port applies from the first start.
- On its first start Paper downloads the matching Mojang server and builds its patched jar into `cache/` and `libraries/`; later starts skip this.
- Reinstall to change version or build. Worlds, plugins and their configs are kept.

Ports, EULA, memory and stop behave as described above. Paper adds `config/paper-*.yml`, `bukkit.yml` and `spigot.yml` with its own settings.

## Purpur

`egg-minecraft-java-purpur.yml` runs [Purpur](https://purpurmc.org/), a fork of Paper with many extra gameplay and configuration options (`purpur.yml`); it loads the same plugins as Paper. It is downloaded from PurpurMC's API and checked against the published MD5.

- Minecraft Version: `latest` for the newest version Purpur supports, or a version such as `1.21.10`. Build Number: `latest` for the newest successful build, or a build number.
- The installer prints the Java version Mojang's metadata gives for the installed version; select that image.
- Like the Paper egg, the installer creates `server.properties` with the port keys, and a reinstall keeps worlds, plugins and configs.

## Folia

`egg-minecraft-java-folia.yml` runs [Folia](https://papermc.io/software/folia), PaperMC's fork of Paper that ticks separate regions of the world in parallel on several CPU cores. It suits servers with many players spread over a large world; only plugins marked as Folia-supported load. Installation, versions, builds and checks work as in the Paper egg (same download service). Folia only exists for recent versions (1.19.4 and newer, Java 17+), so the egg offers Java 17, 21 and 25 images. Give it several CPU cores: `config/paper-global.yml` sets how many threads tick regions.
