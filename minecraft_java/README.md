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
