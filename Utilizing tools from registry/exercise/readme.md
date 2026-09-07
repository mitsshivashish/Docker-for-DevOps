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
