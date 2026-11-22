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

## Velocity (proxy)

`egg-minecraft-java-velocity.yml` runs [Velocity](https://papermc.io/software/velocity), PaperMC's proxy. Players connect to the proxy, which forwards them to the backend servers listed in `velocity.toml` (`[servers]`, `try`) and lets them switch between them. It is downloaded from PaperMC's download service (SHA-256 checked), like Paper; Velocity Version and Build Number work the same way. No EULA is needed for the proxy.

- The panel writes the allocation's port into `bind` and the GameSpy4 query `port` in `velocity.toml` on every start; the installer creates both lines so this works from the first start, and Velocity adds the rest of its settings.
- Backends run on separate servers (for example Paper eggs) that only the proxy should reach. For modern forwarding set `player-info-forwarding-mode = "modern"`, copy `forwarding.secret` to each Paper backend (`config/paper-global.yml`, `proxies.velocity`), and set `online-mode=false` on the backends.
- Plugins go in `plugins/`. Stop sends `end`.

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Proxy | 25577 | TCP | yes (primary allocation) |
| Query | 25577 | UDP | same port; GameSpy4 query |

## BungeeCord / Waterfall (proxy)

`egg-minecraft-java-bungeecord.yml` runs [BungeeCord](https://www.spigotmc.org/wiki/bungeecord/), SpigotMC's proxy, downloaded from its GitHub releases (Build Number `latest` or a build such as `2114`). Set Proxy to `waterfall` for [Waterfall](https://papermc.io/software/waterfall), PaperMC's BungeeCord fork; it reached end of life in 2024 (last version 1.21), so prefer BungeeCord or the Velocity egg for new networks.

- The panel writes the allocation's port into the first listener's `host` and `query_port` in `config.yml` on every start; the installer creates that listener so this works from the first start, and the proxy adds the rest of its settings. Backends are listed under `servers:`.
- Backends need `bungeecord: true` in `spigot.yml` and `online-mode=false`, and should only be reachable by the proxy. Plugins go in `plugins/`. Stop sends `end`.
- Same ports as Velocity (proxy TCP and query UDP on the primary allocation).

## Spigot

`egg-minecraft-java-spigot.yml` runs [Spigot](https://www.spigotmc.org/). SpigotMC only distributes the source, so the installer compiles it with SpigotMC's BuildTools: it downloads a temporary JDK (Eclipse Temurin, the Java version Mojang lists for that Minecraft version, SHA-256 checked) and BuildTools, builds `spigot.jar` and removes the build files again. A build takes a few minutes (about 2 minutes for 26.3 on the test node) and up to 1.5 GB of memory, so give the server at least 2 GB while installing.

- Minecraft Version: `latest` for the newest version on SpigotMC's version list, or a version such as `1.21.10`. Reinstall to switch versions (it compiles again); worlds, plugins and configs are kept.
- Like the Paper egg it creates `server.properties` with the port keys. Plugins go in `plugins/`; Spigot adds `spigot.yml` and `bukkit.yml`.

## Modrinth modpacks

`egg-minecraft-java-modrinth.yml` installs any modpack from [Modrinth](https://modrinth.com/modpacks) (`.mrpack`): set Modrinth Project to the pack's slug or ID (e.g. `adrenaserver`) and Modpack Version to `latest` or a version number.

- The installer downloads the files of the pack's index that servers need (files marked server-unsupported are skipped; every file is checked against its SHA-512), copies `overrides/` and `server-overrides/`, and sets up the pack's loader: Fabric (server launcher), Quilt, Forge or NeoForge (their Java installers run on the first start, as in those eggs), or plain Minecraft.
- It prints the Java version the pack's Minecraft version needs; select that image. Mod loaders and mods are often tied to it: the tested pack (Adrenaserver 1.7.0, Fabric, Minecraft 1.21.1, 83 mods) runs on Java 21 but crashes on Java 25 with a Mixin error at start.
- Reinstall to change the pack version: the files the previous version installed (listed in `.modpack-files`) are removed first; worlds, mods you added yourself and other files stay.
- Like the Paper egg it creates `server.properties` with the port keys.

## FTB modpacks

`egg-minecraft-java-ftb.yml` installs any [Feed The Beast](https://www.feed-the-beast.com/) modpack with FTB's own [server installer](https://github.com/FTBTeam/FTB-Server-Installer) (checked against its published SHA-256). Set FTB Pack ID to the pack's ID (e.g. `135` for FTB Evolution 2) and FTB Version ID to `latest` or a version ID.

- FTB's installer downloads the pack, a matching Java runtime (`jre/`) and the mod loader; the server starts through its `run.sh` with that runtime, whichever Java image is selected. Its recommended `-Xmx` in `user_jvm_args.txt` is replaced on every start by a share of the server's memory limit (95%).
- Packs such as FTB Evolution 2 include the Default Server Properties mod, which applies `default-server.properties` to `server.properties` (without the panel's keys there, the first start listened on 25565); the panel's port keys are kept in both files, so the allocation's port applies from the first start.
- The EULA file FTB's installer writes is removed again, so the panel asks for the Minecraft EULA on the first start.
- Reinstall to update the pack; FTB's installer updates it in place.
- Tested with FTB Evolution 2 1.10.1 (NeoForge, about 1.3 GB of disk, about 7.5 GB of memory after start).

## NanoLimbo

`egg-minecraft-java-nanolimbo.yml` runs [NanoLimbo](https://github.com/Nan1t/NanoLimbo), a limbo server without a world (players wait in an empty dimension), e.g. as an AFK or fallback server behind a proxy. The installer takes a release jar (`latest` or a tag such as `v1.13.0`) and extracts its default `settings.yml`, so the panel's address, port and Max Players apply from the first start (checked with a server list ping: max players 9 on the allocation's port).

- Idle it used about 90 MB of memory and 5 MB of disk on the test node. Stop sends `stop`.
- Player info forwarding from a proxy is set under `infoForwarding` in `settings.yml`.

## Mohist

`egg-minecraft-java-mohist.yml` runs [Mohist](https://mohistmc.com/), a Forge server that also loads Bukkit/Spigot plugins (`mods/` and `plugins/`). The installer downloads a build from MohistMC's API (`latest` or a build number, checked against its SHA-256) for the chosen Minecraft version; the API lists versions from 1.7.10 to 1.21.4, but not every version has builds (1.21.1 had none when tested). Select the Java image that version needs.

- The first start downloads Forge's libraries. Tested: 1.20.1 build 1003 on Java 21, about 2.4 GB of memory after start and 400 MB of disk; `stop` saves the worlds, and the second start works.
- Like the Paper egg it creates `server.properties` with the port keys.

## Arclight

`egg-minecraft-java-arclight.yml` runs [Arclight](https://github.com/IzzelAliz/Arclight), which loads Bukkit/Spigot plugins next to NeoForge, Forge or Fabric mods. Set Minecraft Version and Mod Loader; the installer takes that build from the newest stable GitHub release that has one (or from the release tag given), and lists the available combinations when there is none.

- The first start downloads the loader's libraries. Tested: NeoForge 1.21.1 (FeudalKings/1.0.1) on Java 21, about 1.2 GB of memory after start and 230 MB of disk; `stop` saves the worlds, and the second start works.
- Like the Paper egg it creates `server.properties` with the port keys.

## SpongeVanilla

`egg-minecraft-java-spongevanilla.yml` runs [SpongeVanilla](https://spongepowered.org/), a server for Sponge plugins (`mods/`). The installer takes the newest recommended build from SpongePowered's download API (for the given Minecraft version, or overall with `latest`), or an exact SpongeVanilla version, checked against its SHA-1.

- The first start downloads Sponge's libraries. Tested: 1.21.10-17.0.0 on Java 21, about 1.4 GB of memory after start and 170 MB of disk; `stop` saves the worlds, and the second start works.
- Like the Paper egg it creates `server.properties` with the port keys.

## Cuberite

`egg-cuberite.yml` runs [Cuberite](https://cuberite.org/), a Minecraft-compatible server written in C++ with Lua plugins (`Plugins/`, enabled in `settings.ini`). Clients 1.8 to 1.12.2 can join. The installer takes the newest successful Linux x86-64 build from builds.cuberite.org (checked against its SHA-1); reinstalling updates the program and plugins and keeps existing `.ini` and `.txt` files.

- The panel sets the port, Server Name (list description) and Max Players in `settings.ini` (checked with a server list ping). The installer creates `settings.ini` with these keys and the three default worlds (Cuberite only adds the Nether and the End when it creates the file itself).
- The web admin is off (`webadmin.ini`, `Enabled=0`); to use it, add a user there, enable it and give it a port of an additional allocation.
- Tested: about 150 MB of memory and 24 MB of disk after start; `stop` shuts down cleanly ("Shutdown successful"), and the second start works.

## Technic modpacks (Tekkit and others)

`egg-minecraft-java-technic.yml` installs the server pack of any [Technic Platform](https://www.technicpack.net/) modpack that publishes one (e.g. `tekkitmain`, `tekkit`, `hexxit`, `attack-of-the-bteam`, `blightfall`). Set Technic Pack to the slug from the pack's URL.

- The server jar is the one the pack's start script runs (`.technic-jar`), and that script's `-D` options (such as Blightfall's log4j configuration) are kept (`.technic-java-args`); the memory flags come from the server's limit instead. `-Dlog4j2.formatMsgNoLookups=true` is always added; it only has an effect on log4j 2.10 and newer, so old packs depend on their own log4j configuration.
- Packs for Minecraft before 1.17 need the Java 8 image (the default); both tested packs ran on it.
- Reinstall to update to the pack's newest server pack: files of the previous server pack (`.technic-files`) are removed first, files you added stay, and the world and `server.properties` are never replaced (some server packs ship a world; checked: world files and an edited `server.properties` survived a reinstall).
- Tested: Tekkit (`tekkitmain`, Minecraft 1.6.4) with about 1.8 GB of memory after start and 100 MB of disk, and Blightfall (1.7.10) with about 2.6 GB and 440 MB; `stop` saves the worlds, and the second start works.

## Purpur + Geyser + Floodgate (Java and Bedrock crossplay)

`egg-minecraft-java-purpur-geyser.yml` installs Purpur (as in the Purpur egg, newest build of the chosen version) with the newest [Geyser](https://geysermc.org/) and [Floodgate](https://geysermc.org/wiki/floodgate/) Spigot plugins (checked against their SHA-256; updated on every reinstall). Bedrock players join through Geyser; Floodgate lets them in without a Java account.

- Bedrock needs a second allocation, set as Bedrock Port. Current Geyser (2.12) uses Bedrock's NetherNet transport: its built-in signaling listens on that port over TCP and NetherNet uses it over UDP, so it cannot share the Java port (Geyser then falls back to an external signaling service, which failed to start on the test node). The panel writes the port into `plugins/Geyser-Spigot/config.yml`; the installer creates the keys so this applies from the first start.
- Tested on Purpur 26.3 (Java 25): "Built-in signaling started on http://0.0.0.0:<Bedrock Port>", that port and the Java port reachable from outside, about 1.2 GB of memory and 290 MB of disk; `stop` saves the worlds, and the second start works. A Bedrock client join was not tested.
- RakNet (older Bedrock clients, LAN discovery) can be enabled with `transport: both` in Geyser's config; it needs a third UDP port (`webrtc-port`).
