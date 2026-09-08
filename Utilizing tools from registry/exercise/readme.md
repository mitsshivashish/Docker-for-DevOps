# Exercise 1.11 — Spring

## Dockerfile

    FROM amazoncorretto:8

    WORKDIR /usr/src/app

    COPY . .

    RUN chmod +x mvnw
    RUN ./mvnw package

    EXPOSE 8080

    CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]

## Build the Image

    docker build -t spring-example .

## Run the Container

    docker run -p 8080:8080 spring-example

## Open in Browser

    http://localhost:8080

The application should display a **Success** message.

## Key Points

- `amazoncorretto:8` provides Java 8.
- `WORKDIR` sets the working directory inside the container.
- `COPY . .` copies the Spring project into the container.
- `./mvnw package` builds the Spring application.
- `EXPOSE 8080` documents the application's port.
- `-p 8080:8080` maps the container's port to the host.
- `CMD` starts the generated JAR file.




# Exercise 1.12 — Hello Frontend

## Project

Example Frontend:

https://github.com/docker-hy/material-applications/tree/main/example-frontend

The task is to containerize the project using **Ubuntu as the base image** without modifying the project code.

## Dockerfile

    FROM ubuntu:latest

    WORKDIR /usr/src/app

    # Install required packages
    RUN apt-get update && apt-get install -y \
        curl \
        nodejs \
        npm

    # Copy project files
    COPY . .

    # Install project dependencies
    RUN npm install

    # Start the application
    CMD ["npm", "start"]

    EXPOSE 5001

## Build the Image

    docker build -t example-frontend .

## Run the Container

    docker run -p 5001:5001 example-frontend

## Open in Browser

    http://localhost:5001

The application should display the success message once it has started accepting connections.

## Important Notes

- Ubuntu must be used as the base image.
- The application may take a few seconds to start.
- Wait until you see:

      Accepting connections at http://localhost:5001

- The project may not work with the newest Node.js versions.
- Nothing needs to be installed outside the container.

## Key Dockerfile Instructions

| Instruction | Purpose |
|---|---|
| `FROM ubuntu:latest` | Uses Ubuntu as the base image |
| `WORKDIR` | Sets the working directory |
| `RUN` | Installs required software and dependencies |
| `COPY` | Copies the project into the image |
| `EXPOSE 5001` | Documents the application port |
| `CMD` | Starts the frontend application |

## Commands Summary

    docker build -t example-frontend .
    docker run -p 5001:5001 example-frontend

![Dockerfile](./images/dockerfile-for-hellofrontend.png)
![output](./images/output-for-hellofrontend.png)
