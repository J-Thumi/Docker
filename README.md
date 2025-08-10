# Docker

## What is Docker?

Docker is a platform for developing, packaging, and running applications in containers.
Without Docker → You install your app, dependencies, and configurations directly on your computer or server. This can cause _"works on my machine"_ problems.

With Docker → You package everything the app needs (code, runtime, libraries, environment variables, etc.) into a lightweight, portable container. You can run that container anywhere — no need to reconfigure.

***

## Key Concepts
### Image
A blueprint for a container.
Contains your application code + all the dependencies + environment configuration.
Created from a **Dockerfile** (a script with instructions for building the image).
Example: python:3.11, nginx:latest.

### Container
A running instance of an image.
You can have multiple containers from the same image, each isolated.
Containers are ephemeral — they can be stopped, deleted, or replaced quickly.

### Dockerfile 
A text file with instructions for building an image. Defines how to build an image.Think of it as a recipe: it lists instructions to create the environment (install dependencies, copy code, set configs).It is used for creating a Docker image.

Example: 
```
# Dockerfile
FROM node:20
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "start"]
```

### Docker-compose.yml
Defines how to run one or more containers together.Think of it as the dinner menu & serving plan: it specifies what containers to run, their environment variables, networks, volumes, and dependencies.It is used for Running multiple services (like app + database) with one command.

Example:

```version: "3.8"
services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb
    ports:
      - "3306:3306"
```

### Docker Hub
A public (or private) registry where Docker images are stored and shared.
Like GitHub for code, but for container images.

### Volumes
A way to persist data outside the container.
Because containers are disposable, volumes let you keep things like databases between restarts.

### Networks
Containers can communicate with each other over Docker networks.
Useful in microservices setups (e.g., web service container talking to a database container).

### Layers 
Every instruction in a Dockerfile creates a layer. Layers are cached so rebuilds are faster.

### Base Image
The starting point (e.g., python:3.12, golang:1.21-alpine, or ubuntu:22.04)

### Build Context 
The directory you run docker build from. All files inside are available to the Docker build.

### Caching 
If nothing changes in a layer, Docker uses the cached version to speed up builds.

### Small Image Size 
Use minimal images (e.g., alpine) to reduce size and security risks.

## Writing a Dockerfile

A Dockerfile is simply a text file containing a set of instructions for building a Docker image.
You tell Docker:

* Which base OS or base image to start from
* Which files to copy in
* Which dependencies to install
* Which commands to run
* Which ports to expose
* What command to execute when the container starts

###  Common Dockerfile Instructions

| Instruction     | Purpose                                                     | Example                                         |
| --------------- | ----------------------------------------------------------- | ----------------------------------------------- |
| `FROM`          | Set base image                                              | `FROM node:20-alpine`                           |
| `WORKDIR`       | Set working directory                                       | `WORKDIR /app`                                  |
| `COPY`          | Copy files from host to image                               | `COPY . /app`                                   |
| `ADD`           | Like `COPY` but can also extract archives and download URLs | `ADD app.tar.gz /app`                           |
| `RUN`           | Execute commands in the image during build                  | `RUN apt-get update && apt-get install -y curl` |
| `ENV`           | Set environment variables                                   | `ENV PORT=8080`                                 |
| `EXPOSE`        | Declare the port the app listens on                         | `EXPOSE 8080`                                   |
| `CMD`           | Default command when container starts                       | `CMD ["node", "server.js"]`                     |
| `ENTRYPOINT`    | Similar to CMD but always runs first                        | `ENTRYPOINT ["python"]`                         |
| `ARG`           | Pass build-time variables                                   | `ARG NODE_VERSION=20`                           |
| `.dockerignore` | File to exclude files from build                            | `node_modules`                                  |


### Best Practices
* Minimize layers – Combine `RUN` commands with &&
* Use .dockerignore – Prevent copying unnecessary files (like node_modules)
* Pin versions – Avoid "latest" for reproducibility
* Use multi-stage builds – Compile in one stage, copy only final output into a smaller image
* Keep images small – Use Alpine or slim versions
* Separate config from code – Use `ENV` or bind mounts


###  The Dockerfile Build Process

`docker build -t myimage .` OR `docker build -t myimage:1.0 .`

`myimage` is the name of the image.
`tag` is a label for that image version (e.g., `v1.0`, `latest`, `dev`).
The `.` means “build using the Dockerfile in the current directory.”

**Tagging** is optional, but important for versioning.
Without a tag, it defaults to `:latest`, which can get confusing when managing multiple builds.