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
