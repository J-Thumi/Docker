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