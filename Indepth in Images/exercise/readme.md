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
