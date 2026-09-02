# Docker - Running Containers

## Run Ubuntu

    docker run ubuntu

Downloads the Ubuntu image if it is not available locally and starts a container.

## Interactive Containers

    docker run -t ubuntu      # -t → TTY
    docker run -i ubuntu      # -i → Interactive
    docker run -it ubuntu     # -it → Interactive + TTY

Exit the container:

    exit

## Detached Mode

Run a container in the background:

    docker run -d nginx

`-d` → Detached mode

## Name a Container

    docker run -d -it --name looper ubuntu sh -c 'while true; do date; sleep 1; done'

`--name` → Assigns a custom name to the container.

## View Logs

    docker logs -f looper

`-f` → Follow logs continuously

## Pause / Unpause

    docker pause looper
    docker unpause looper

## Attach to a Container

    docker attach looper

Detach without stopping the container:

    Ctrl + P, Ctrl + Q

Attach without connecting STDIN:

    docker attach --no-stdin looper

`Ctrl + C` while attached can stop the container if it terminates the main process.

## Start / Stop / Kill

    docker start <container>
    docker stop <container>
    docker kill <container>

`docker stop` sends SIGTERM first and may send SIGKILL after the grace period.

## Execute Commands Inside a Running Container

Run a command:

    docker exec looper ls -la

Open an interactive shell:

    docker exec -it looper bash

## Automatically Remove Containers

    docker run -d --rm -it --name looper-it ubuntu sh -c 'while true; do date; sleep 1; done'

`--rm` → Automatically removes the container after it exits.

> A container started with `--rm` cannot be restarted with `docker start` after it exits.

## Remove Containers

    docker rm <container>

Force remove:

    docker rm --force <container>

Example:

    docker kill looper && docker rm looper

## Docker Flags

| Flag | Meaning |
|---|---|
| `-i` | Interactive / keep STDIN open |
| `-t` | Allocate TTY |
| `-it` | Interactive + TTY |
| `-d` | Detached/background |
| `--name` | Custom container name |
| `--rm` | Automatically remove container |
| `-f` | Follow logs |

## Ubuntu Container

Start Ubuntu interactively:

    docker run -it ubuntu

Install packages inside the container:

    apt-get update
    apt-get -y install nano

> Software installed inside a container is removed when the container is removed unless it is included in a new image or otherwise persisted.

## Platform Mismatch

On ARM systems such as M1/M2 Macs, an `amd64` image may show a platform mismatch warning.

Docker Desktop can use emulation, but performance may be lower.

Many popular images are **multi-platform images**, allowing Docker to automatically select a compatible architecture.

## Quick Cheatsheet

    docker run ubuntu
    docker run -it ubuntu
    docker run -d nginx

    docker ps
    docker logs -f <container>
    docker pause <container>
    docker unpause <container>
    docker attach <container>
    docker exec <container> <command>
    docker exec -it <container> bash

    docker start <container>
    docker stop <container>
    docker kill <container>
    docker rm <container>

    docker run --rm <image>

> **Remember:** `-i` = interactive, `-t` = TTY, `-d` = background, `--name` = custom name, `--rm` = auto-remove.
