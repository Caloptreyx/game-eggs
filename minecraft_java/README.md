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

## Fabric

`egg-minecraft-java-fabric.yml` runs Minecraft with the [Fabric](https://fabricmc.net/) mod loader. The installer downloads Fabric's server launcher from `meta.fabricmc.net`; on its first start the launcher downloads the official Mojang server and the loader libraries (`.fabric/`, `libraries/`).

- Minecraft Version: `latest` for the newest stable version Fabric supports, `snapshot`, or a version such as `1.21.10`. Fabric Loader Version: `latest` for the newest stable loader, or a fixed one.
- Put mods (`.jar`, built for Fabric and the installed Minecraft version) in `mods/`; most mods need [Fabric API](https://modrinth.com/mod/fabric-api) there too. The log lists the loaded mods (`Loading N mods`).
- The installer prints the Java version Mojang's metadata gives for the installed version; select that image. Like the Paper egg it creates `server.properties` with the port keys, and a reinstall keeps worlds, mods and configs.

## Quilt

`egg-minecraft-java-quilt.yml` runs Minecraft with the [Quilt](https://quiltmc.org/) mod loader, which loads Quilt mods and most Fabric mods (put them in `mods/`). Quilt's installer is a Java program and the installer image has no Java, so the install script only picks the versions and downloads the installer (checked against its SHA-256); the first start after each (re)install runs it (`Installing Quilt ...` in the console), which downloads the Mojang server and the loader libraries, and then starts the server through `quilt-server-launch.jar`.

- Minecraft Version: `latest` for the newest stable version Quilt supports, `snapshot`, or a version such as `1.21.10`. Quilt Loader Version: `latest` for the newest loader release (betas are skipped), or a fixed one.
- Java image as printed by the installer (Quilt needs Java 17 or newer). Worlds, mods and configs survive reinstalls.

## Forge

`egg-minecraft-java-forge.yml` runs Minecraft with [Minecraft Forge](https://files.minecraftforge.net/), for Forge mods (put them in `mods/`). The install script picks the versions from Forge's promotions list and downloads Forge's installer from its Maven (checked against the published SHA-1). The installer is a Java program and the installer image has no Java, so the first start after each (re)install runs it (`Installing Forge ...`, its output goes to `forge-installer.log`); it downloads the Mojang server and the libraries. Forge 1.17 and newer start through the argument file in `libraries/net/minecraftforge/forge/<version>/unix_args.txt`, older versions through their `forge-<version>.jar`; the startup command picks the right one.

- Minecraft Version: `latest` for the newest version Forge has a build for, or a version such as `1.20.1` or `1.12.2`.
- Forge Version: `recommended` (the recommended build, or the latest if none is recommended), `latest`, or a Forge version such as `47.4.0`.
- Java: as printed by the installer; old versions need old Java (Forge for 1.12.2 needs the Java 8 image). Tested: 26.3 with Forge 66.1.9 on Java 25 and 1.12.2 with Forge 14.23.5.2859 on Java 8.
- Worlds, mods and configs survive reinstalls.

## NeoForge

`egg-minecraft-java-neoforge.yml` runs Minecraft with [NeoForge](https://neoforged.net/), for NeoForge mods (put them in `mods/`). Like the Forge egg, the install script downloads NeoForge's installer from its Maven (SHA-1 checked) and the first start after each (re)install runs it (`Installing NeoForge ...`, output in `neoforge-installer.log`); the server then starts through `libraries/net/neoforged/neoforge/<version>/unix_args.txt`.

- Minecraft Version: `latest`, or a version such as `1.21.1`; the egg picks the newest NeoForge release (betas excluded) for it. NeoForge Version: `latest`, or a fixed version such as `21.1.267`, which also decides the Minecraft version (21.1.x is for 1.21.1, 26.3.0.x for 26.3).
- NeoForge for 1.20.1 was a fork of Forge with different coordinates and is not offered; use the Forge egg for 1.20.1.
- Java 17, 21 or 25 image, as printed by the installer. Tested: 26.3 (NeoForge 26.3.0.115) and 1.21.1 (NeoForge 21.1.267) on Java 25. Worlds, mods and configs survive reinstalls.
