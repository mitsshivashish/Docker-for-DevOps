# Docker - Basics

## What is Docker?

Docker is a set of tools used to deliver software in **containers**.

**Container = Application + Dependencies + Isolated Environment**

## Benefits of Containers

- **Works on my machine** → Same application environment across systems
- **Isolation** → Different applications can use different dependency versions
- **Development** → Easily run services like PostgreSQL, MongoDB, Redis
- **Scaling** → Start, stop, and replace containers quickly

## Docker vs Virtual Machines

| Containers | Virtual Machines |
|---|---|
| Share host OS kernel | Each VM has a full OS |
| Lightweight | Heavier |
| Faster startup | Slower startup |
| Process-level isolation | Stronger isolation |
| Better resource utilization | Higher resource usage |

## Image vs Container

- **Image** → Immutable blueprint/template
- **Container** → Running instance of an image

    Dockerfile → Image → Container

## Dockerfile

A Dockerfile contains instructions used to build an image.

    FROM <image>:<tag>
    RUN <install dependencies>
    CMD <command>

## Running Containers

    docker container run hello-world
    docker run hello-world

Run in background:

    docker run -d nginx

`-d` → Detached/background mode

## Images

    docker image ls
    docker image pull <image>
    docker image rm <image>

Shortcuts:

    docker images
    docker pull <image>
    docker rmi <image>

## Containers

    docker container ls
    docker container ls -a
    docker container stop <container>
    docker container rm <container>
    docker container exec <container> <command>

Shortcuts:

    docker ps
    docker ps -a
    docker stop <container>
    docker rm <container>
    docker exec <container> <command>

### Container Notes

- `docker ps` shows only running containers
- `docker ps -a` shows all containers
- Containers have a **CONTAINER ID** and **NAME**
- A container can be referenced by name, full ID, or unique ID prefix
- `docker run` creates a new container from an image
- Multiple containers can be created from the same image
- A stopped container still exists until removed
- A running container must be stopped before normal removal

## Cleanup

    docker container prune
    docker image prune
    docker system prune

- `container prune` → Removes stopped containers
- `image prune` → Removes dangling images
- `system prune` → Removes unused Docker resources

## Docker CLI Architecture

Docker has:

    Docker CLI
        ↓
      REST API
        ↓
    Docker Daemon

The **Docker daemon** manages containers, images, and other Docker resources.

## Quick Cheatsheet

| Task | Command |
|---|---|
| Run container | `docker run <image>` |
| Run in background | `docker run -d <image>` |
| List images | `docker image ls` |
| Pull image | `docker image pull <image>` |
| Remove image | `docker image rm <image>` |
| Running containers | `docker ps` |
| All containers | `docker ps -a` |
| Stop container | `docker stop <container>` |
| Remove container | `docker rm <container>` |
| Execute command | `docker exec <container> <command>` |
| Remove stopped containers | `docker container prune` |
| Remove dangling images | `docker image prune` |
| System cleanup | `docker system prune` |

> **Remember:** Dockerfile builds an **Image**, and an Image is used to create a **Container**. Containers package applications with their dependencies in an isolated environment.
