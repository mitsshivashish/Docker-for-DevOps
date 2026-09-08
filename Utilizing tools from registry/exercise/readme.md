# Containerizing a Rails Project

This exercise demonstrates how to containerize an existing Rails application by converting the setup instructions from its README into a Dockerfile.

## Project

Rails example project:

https://github.com/docker-hy/material-applications/tree/main/rails-example-project

## Dockerfile

    # Use Ruby 3.1.0
    FROM ruby:3.1.0

    # Application runs on port 3000
    EXPOSE 3000

    # Set working directory
    WORKDIR /usr/src/app

    # Install the required Bundler version
    RUN gem install bundler:2.3.3

    # Copy dependency files first
    COPY Gemfile* ./

    # Install Ruby dependencies
    RUN bundle install

    # Copy the rest of the project
    COPY . .

    # Run database migrations in production
    RUN rails db:migrate RAILS_ENV=production

    # Precompile assets
    RUN rake assets:precompile

    # Start the application in production mode
    CMD ["rails", "s", "-e", "production"]

## Build and Run

    docker build . -t rails-project && docker run -p 3000:3000 rails-project

The application will be available at:

    http://localhost:3000/

## Why COPY Gemfile* Before COPY . .

    COPY Gemfile* ./
    RUN bundle install
    COPY . .

This separates dependency installation from the application source code.

Docker can cache the dependency layers, so when only the source code changes, Docker does not need to reinstall all Ruby dependencies.

The same caching idea can be used with other technologies such as Node.js.

## M-Series Mac Issue

On newer Macs with Apple M-series processors, building the image may fail during:

    RUN rails db:migrate RAILS_ENV=production

The error can be related to `nokogiri`.

The course material suggests changing `Gemfile.lock`:

    nokogiri (1.13.1-x86_64-darwin)

to:

    nokogiri (1.14.2-arm64-darwin)

The issue occurs because `Gemfile.lock` contains platform-specific dependency information and the original lock file was generated for an Intel-based system.

## Prebuilt Docker Images

Instead of starting from a basic image such as Ubuntu and manually installing Ruby, the Dockerfile uses:

    FROM ruby:3.1.0

This is a prebuilt Docker image available on Docker Hub that already contains Ruby and its runtime environment.

### Benefits

- Simpler setup
- Ruby is already installed
- Less manual configuration
- Faster and easier Dockerfile creation

## Key Dockerfile Instructions

| Instruction | Purpose |
|---|---|
| `FROM` | Selects the base image |
| `EXPOSE` | Documents the port used by the application |
| `WORKDIR` | Sets the working directory inside the container |
| `RUN` | Executes commands while building the image |
| `COPY` | Copies files into the image |
| `CMD` | Defines the command used to start the container |

## Important Concept

The README tells us:

    What to install
    What dependencies are required
    How to build the application
    How to run the application

As container experts, we translate those instructions into Dockerfile commands.

### Mental Model

    README instructions
            ↓
       Dockerfile
            ↓
       Docker Image
            ↓
        Container
            ↓
      Running Rails App
