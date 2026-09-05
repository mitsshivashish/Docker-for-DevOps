# Docker - Volumes & Bind Mounts

## Volumes

Volumes allow files/directories to be shared between the **host machine and containers**.

Data stored in a volume is preserved even after the container is stopped or removed.

### Bind Mount

Use `-v` to mount a host path into a container:

    docker run -v "$(pwd):/mydir" yt-dlp https://www.youtube.com/watch?v=saEpkcVi1d4

- `$(pwd)` → Current directory on host
- `/mydir` → Directory inside container
- Downloaded files are saved directly to the host directory.

## Mount a Single File

    docker run -v "$(pwd)/material.md:/mydir/material.md" <image>

Changes made to `material.md` on the host are reflected inside the container, and vice versa.

> ⚠️ If the specified host file does not exist, `-v` may create a directory at that path instead.

## Key Point

    Host Directory/File
            ↓
        Bind Mount
            ↓
    Container Directory/File

Without a volume/bind mount, files created inside the container's ephemeral storage can be lost when the container is removed.

> **Remember:** `-v <host-path>:<container-path>` connects host storage with the container and allows data to persist outside the container.


# Docker - Allowing External Connections

## localhost & 127.0.0.1

`localhost` and `127.0.0.1` refer to the **same machine/container** where the request originates.

Example:

    http://localhost:3000

- `http` → Protocol
- `localhost` / `127.0.0.1` → Host
- `3000` → Port

## Port Mapping

You can map a **host port** to a **container port**:

    -p <host-port>:<container-port>

Example:

    -p 1000:2000

Then:

    http://localhost:1000

connects to port `2000` inside the container.

## Expose vs Publish

### EXPOSE

Declare the port used by the application in the `Dockerfile`:

    EXPOSE <port>

This mainly documents that the container listens on that port.

### Publish

Actually map the port when starting the container:

    docker run -p <host-port>:<container-port> <image>

Example:

    docker run -p 8080:8080 <image>

## Automatic Host Port

You can omit the host port:

    docker run -p 4567 <image>

Docker automatically selects a free host port and maps it to container port `4567`.

## Protocol

Ports can be restricted to a protocol:

    EXPOSE 3000/udp

    docker run -p 3000:3000/udp <image>

## Security

Publishing:

    -p 3456:3000

is equivalent to:

    -p 0.0.0.0:3456:3000

This can allow connections from outside your machine.

For local-only access:

    docker run -p 127.0.0.1:3456:3000 <image>

Now only your machine can access the container through port `3456`.

> **Remember:** `EXPOSE` documents a container port, while `-p` publishes/maps a container port to the host. Use `127.0.0.1:<host>:<container>` when you want to restrict access to your own machine.
