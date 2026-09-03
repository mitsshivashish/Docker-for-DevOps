# Docker Exercise 1.3 - Secret Message

## Question

Run `devopsdockeruh/simple-web-service:ubuntu`, enter the running container, and follow `./text.log` using `tail -f`.

Every 10 seconds, a **secret message** appears.

## Answer

### Commands Used

    docker run -d --name logger_mode devopsdockeruh/simple-web-service:ubuntu
    docker exec -it logger_mode bash
    tail -f ./text.log

### Output

![Secret message output](./images/secret-message.png)



# Docker Exercise 1.4 - Missing Dependencies

## Question

Start an Ubuntu container and run the given script. The script uses `curl` to access a website, but `curl` is missing.

## Answer

### i) Command Used to Start the Process

    docker run -it ubuntu sh -c 'while true; do echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website; done'

### ii) Commands Used to Fix the Problem

Install `curl` inside the running container:

    docker exec -it <container_id_or_name> bash

    apt-get update
    apt-get install -y curl

Then enter:

    helsinki.fi

### Output

### docker exec -it logger_mode bash

![Initial output](./images/missing-curl.png)

### apt-get update

![command 1](./images/apt-update.png)

### apt-get install -y curl

![command 2](./images/curl-install.png)

### sh -c 'while true; do echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website; done'

![Fixed output](./images/curl-working.png)



# Docker Exercise 1.7 - Image for Script

## Question

Create a Docker image based on `ubuntu:24.04` that:

1. Installs `curl`
2. Copies `script.sh` into the image
3. Runs the script when the container starts
4. Builds the image with the name `curler`

## `script.sh`

    #!/bin/bash
    while true
    do
      echo "Input website:"
      read website; echo "Searching.."
      sleep 1; curl http://$website
    done

## Dockerfile

    FROM ubuntu:24.04

    RUN apt-get update && apt-get install -y curl

    COPY script.sh /script.sh

    RUN chmod +x /script.sh

    CMD ["/script.sh"]

## Build & Run

    docker build -t curler .
    docker run -it curler

## Output

![Dockerfile](./images/dockerfile.png)

![Build](./images/docker-build.png)

![Run](./images/docker-run.png)

![Output](./images/docker-output.png)

> **Remember:** `FROM` → base image, `RUN` → install/setup, `COPY` → add files, `CMD` → command executed when the container starts.
