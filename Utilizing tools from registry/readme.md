# 🚂 Ruby on Rails Containerisation Guide

This guide covers how to interpret a project README to containerise an application using pre-built base images and an optimised Docker multi-layer layout.

---

## 🛠️ Step 1: Base Configurations

Start by placing the `Dockerfile` in the project root folder. We deduce the framework foundations and port definitions directly from the project's setup instructions.

```dockerfile
# We need ruby 3.1.0. I found this from Docker Hub
FROM ruby:3.1.0

EXPOSE 3000

WORKDIR /usr/src/app
```

* **`FROM ruby:3.1.0`**: We use a pre-built programming language image instead of starting from a plain base like Ubuntu. This simplifies things because the runtime environment is already configured.
* **`EXPOSE 3000`**: Documenting the application communication gateway specified at the bottom of the README.
* **`WORKDIR /usr/src/app`**: Setting up the standard working directory convention.

---

## ⚡ Step 2: Optimised Dependency Installation

To avoid redundant asset fetching when application code changes, extract the environment setup steps from your README instructions and use a layer caching optimization:

```dockerfile
# Install the correct bundler version
RUN gem install bundler:2.3.3

# Copy the files required for dependencies to be installed
COPY Gemfile* ./

# Install all dependencies
RUN bundle install
```

### 💡 The Layer Caching Trick
By running `COPY Gemfile* ./` and `RUN bundle install` **before** copying the rest of your source code, Docker caches these dependency layers. If you update your website's source code later, Docker reuses the cached packages instead of wasting time downloading libraries all over again. This same optimization approach applies to other ecosystems like Node.js.

---

## 🚀 Step 3: Copying Code and Pre-deployment Setup

Once the dependencies are handled, copy your underlying application logic and execute the production commands specified in your instructions:

```dockerfile
# Copy all of the source code
COPY . .

# We pick the production mode since we have no intention of developing the software inside the container.
# Run database migrations by following instructions from README
RUN rails db:migrate RAILS_ENV=production

# Precompile assets by following instructions from README
RUN rake assets:precompile

# And finally the command to run the application
CMD ["rails", "s", "-e", "production"]
```

---

## 🏃 Step 4: Build and Run with One Command

Execute the following chained one-liner in your terminal to build the blueprint asset and execute the app with port 3000 mapped:

```bash
docker build . -t rails-project && docker run -p 3000:3000 rails-project
```

Once execution finishes, navigate to the web service portal using your host machine browser:
👉 **`http://localhost:3000/`**

---

## 🍏 Troubleshooting: Building on Apple M-Series Macs

If you run the build workflow using a newer Mac equipped with an Apple Silicon processor (M1, M2, M3, M4, etc.), database migrations might fail with this error block:

```text
 => ERROR [7/8] RUN rails db:migrate RAILS_ENV=production
------
 > [7/8] RUN rails db:migrate RAILS_ENV=production:
#11 1.142 rails aborted!
#11 1.142 LoadError: cannot load such file -- nokogiri
```

### Why it Happens
The lock file `Gemfile.lock` defines specific package versions and was originally generated on an Intel-based Linux platform. The package **Nokogiri** requires architecture-specific binaries for Intel systems vs. Apple ARM systems. 

### The Fix
Open up your **`Gemfile.lock`** workspace configuration in a text editor and change the string manually:

* **Find this:** `nokogiri (1.13.1-x86_64-darwin)`
* **Replace with this:** `nokogiri (1.14.2-arm64-darwin)`

Save your updates and restart the image build workflow.

