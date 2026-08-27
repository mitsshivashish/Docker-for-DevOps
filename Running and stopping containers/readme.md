# Docker - Running Containers

## Run Ubuntu

    docker run ubuntu

Downloads the image if it is not available locally and runs a container.

## Interactive Containers

### `-t` → TTY

    docker run -t ubuntu

### `-i` → Interactive

    docker run -i ubuntu

### `-it` → Interactive + TTY

    docker run -it ubuntu

Inside the container:

    ls
    exit

### `-d` → Detached

Runs the container in the background.

    docker run -d nginx

## Name a Container

Use `--name` to give a container an easy-to-remember name.

    docker run -d -it --name looper ubuntu sh -c 'while true; do date; sleep 1; done'

## View Logs

Follow container output:

    docker logs -f looper

## Pause / Resume

    docker pause looper
    docker unpause looper

## Attach to a Container

Attach to its output:

    docker attach looper

Detach without stopping it:

    Ctrl+P, Ctrl+Q

To attach without connecting STDIN:

    docker attach --no-stdin looper

`Ctrl+C` while attached normally sends a signal to the container's main process and may stop it.

## Start / Stop / Kill

    docker start <container>
    docker stop <container>
    docker kill <container>

`docker stop` sends SIGTERM first and may follow with SIGKILL after the grace period.

## Execute Commands Inside a Running Container

Run a single command:

    docker exec looper ls -la

Open an interactive shell:

    docker exec -it looper bash

## Automatically Remove Container

Use `--rm` to delete the container automatically after it exits.

    docker run -d --rm -it --name looper-it ubuntu sh -c 'while true; do date; sleep 1; done'

> With `--rm`, the container cannot be started again with `docker start` after it exits.

## Remove Container

    docker rm <container>

Force remove:

    docker rm --force <container>

Example:

    docker kill looper && docker rm looper

## Useful Flags

| Flag | Purpose |
|---|---|
| `-i` | Interactive / keep STDIN open |
| `-t` | Allocate a TTY |
| `-it` | Interactive + TTY |
| `-d` | Detached/background mode |
| `--name` | Assign container name |
| `--rm` | Automatically remove after exit |
| `-f` | Follow logs / context dependent |
| `-3` | Not applicable here |

## Ubuntu in a Container

An Ubuntu container behaves like a lightweight Ubuntu environment.

    docker run -it ubuntu

Install packages normally:

    apt-get update
    apt-get -y install nano

Changes made inside the container are lost when the container is removed unless persisted using other Docker mechanisms.

## Platform Mismatch

On ARM-based systems, an image built for `linux/amd64` may show a platform warning.

Docker can use emulation, but performance may be lower.

Many popular images are **multi-platform images**, so Docker automatically selects the appropriate architecture.

## Quick Cheatsheet

    docker run ubuntu
    docker run -it ubuntu
    docker run -d nginx
    docker run -d -it --name looper ubuntu sh -c 'while true; do date; sleep 1; done'

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
