# Docker - Images & Building Images

## Docker Hub

When an image is not available locally, Docker automatically searches **Docker Hub**.

    docker run hello-world
    docker run postgres

Search for images:

    docker search <image>

Example:

    docker search hello-world

Official images are marked with `[OK]` in the `OFFICIAL` column and usually have no user/organization prefix.

Images can also come from other registries:

    docker pull quay.io/podman/hello

## Image Names & Tags

General format:

    registry/organization/image:tag

Short form:

    ubuntu

Defaults:

    Registry       → Docker Hub
    Organization   → library
    Tag            → latest

Specify a tag:

    docker pull ubuntu:25.10

Tag an image locally:

    docker tag ubuntu:25.10 ubuntu:questing_quokka

Tags can be used to maintain different versions of the same image.

## Image Layers

Docker images are made of **layers**.

Layers:
- Can be downloaded in parallel
- Help reduce repeated work
- Are cached during builds
- Allow Docker to rebuild only changed layers

## Building an Image

A `Dockerfile` contains instructions for building an image.

Example:

    FROM alpine:3.21
    WORKDIR /usr/src/app
    COPY hello.sh .
    CMD ["./hello.sh"]

Build the image:

    docker build -t hello-docker .

Run it:

    docker run hello-docker

Check images:

    docker image ls

## Important Dockerfile Instructions

    FROM    → Base image
    WORKDIR → Working directory
    COPY    → Copy files into image
    RUN     → Execute command during build
    CMD     → Default command when container starts

> `RUN` executes during **build time**, while `CMD` executes during **container runtime**.

## Dockerfile Best Practice

Prefer defining changes in the `Dockerfile` instead of manually modifying a container and using `docker commit`.

This makes images:
- Reproducible
- Version controlled
- Easier to maintain
- Easier to rebuild

## Container Changes

Copy a file into a running container:

    docker cp ./additional.txt <container>:/usr/src/app/

View filesystem changes:

    docker diff <container>

Change indicators:

    A → Added
    D → Deleted
    C → Changed

## Docker Commit

Create a new image from a container:

    docker commit <container> <new-image>

Example:

    docker commit container1 hello-docker-additional

> `docker commit` creates a new image layer, but using a `Dockerfile` is the preferred approach for maintaining image changes.

## Multiple Dockerfiles

You can have multiple Dockerfiles in the same project:

    Dockerfile
    Dockerfile.testing
    Dockerfile.production

Build using a specific Dockerfile:

    docker build -t tester -f Dockerfile.testing .

## Quick Cheatsheet

| Task | Command |
|---|---|
| Search Docker Hub | `docker search <image>` |
| Pull image | `docker pull <image>` |
| Pull specific tag | `docker pull <image>:<tag>` |
| Tag image | `docker tag <source> <target>` |
| List images | `docker image ls` |
| Build image | `docker build -t <name> .` |
| Build with specific Dockerfile | `docker build -t <name> -f <Dockerfile> .` |
| Run image | `docker run <image>` |
| Copy file to container | `docker cp <file> <container>:<path>` |
| Check container changes | `docker diff <container>` |
| Create image from container | `docker commit <container> <image>` |

> **Remember:** Dockerfile → Image → Container. Use tags for versions, layers for efficient builds, and Dockerfiles for reproducible image creation.
