# Docker Complete Reference Guide

*A practical handbook for beginners and experienced engineers*

---

# Table of Contents

1. What is Docker?
2. Docker Mental Model
3. Docker Architecture
4. Images
5. Containers
6. Dockerfile
7. Volumes
8. Networks
9. Environment Variables
10. Port Mapping
11. Docker Compose
12. Registries
13. Resource Management
14. Logs
15. Debugging
16. Security
17. Cleanup Operations
18. Production Best Practices
19. Common Workflows
20. Troubleshooting
21. Command Cheat Sheet

---

# 1. What is Docker?

Docker is a platform that packages an application together with:

* Code
* Runtime
* Libraries
* Dependencies
* Configuration

into a portable unit called a **Container**.

Without Docker:

```text
Developer Machine
├── Java 21
├── Maven
├── PostgreSQL
├── Redis
├── NodeJS
└── Works on my machine
```

Production:

```text
Java 17
Node 18
Different Libraries

=> Application fails
```

With Docker:

```text
Application
+
Dependencies
+
Runtime
=
Container
```

Runs the same everywhere.

---

# 2. Docker Mental Model

Think of Docker like shipping containers.

Real World:

```text
Cargo
    ↓
Container
    ↓
Ship
```

Docker:

```text
Application
    ↓
Image
    ↓
Container
```

---

## Core Concepts

### Image

Blueprint.

Example:

```text
Ubuntu
Java 21
My App Jar
```

Not running.

---

### Container

Running instance of an image.

Example:

```bash
docker run nginx
```

Creates a running container.

---

### Dockerfile

Recipe to create an image.

---

### Registry

Image storage.

Examples:

* Docker Hub
* ECR
* GCR
* ACR

---

### Volume

Persistent storage.

---

### Network

Communication channel between containers.

---

# 3. Docker Architecture

```text
Docker CLI
      |
      v
Docker Daemon
      |
      +--> Images
      +--> Containers
      +--> Networks
      +--> Volumes
```

---

## Components

### Docker Client

```bash
docker
```

CLI.

---

### Docker Daemon

```text
dockerd
```

Actual engine.

---

### Registry

Stores images.

---

### Objects

* Images
* Containers
* Volumes
* Networks

---

# 4. Images

---

## Pull Image

```bash
docker pull nginx
```

Specific version:

```bash
docker pull nginx:1.27
```

---

## List Images

```bash
docker images
```

or

```bash
docker image ls
```

---

## Remove Image

```bash
docker rmi nginx
```

Force:

```bash
docker rmi -f nginx
```

---

## Inspect Image

```bash
docker inspect nginx
```

---

## History

```bash
docker history nginx
```

Shows image layers.

---

# Image Tags

```text
nginx:latest
```

Format:

```text
image:tag
```

Examples:

```text
java:21
postgres:17
redis:8
```

---

# 5. Containers

---

## Run Container

```bash
docker run nginx
```

---

## Detached Mode

```bash
docker run -d nginx
```

Runs in background.

---

## Named Container

```bash
docker run --name web nginx
```

---

## Interactive Container

```bash
docker run -it ubuntu bash
```

Flags:

```text
-i = interactive
-t = terminal
```

---

## List Running Containers

```bash
docker ps
```

---

## List All Containers

```bash
docker ps -a
```

---

## Stop Container

```bash
docker stop container_id
```

---

## Start Container

```bash
docker start container_id
```

---

## Restart

```bash
docker restart container_id
```

---

## Remove Container

```bash
docker rm container_id
```

Force:

```bash
docker rm -f container_id
```

---

## Rename Container

```bash
docker rename old new
```

---

## Inspect Container

```bash
docker inspect container_id
```

---

# Container Lifecycle

```text
Created
   ↓
Running
   ↓
Paused
   ↓
Stopped
   ↓
Removed
```

---

# 6. Dockerfile

Build recipe.

Example:

```dockerfile
FROM eclipse-temurin:21-jre

WORKDIR /app

COPY target/app.jar app.jar

EXPOSE 8080

CMD ["java","-jar","app.jar"]
```

---

## Build Image

```bash
docker build -t myapp .
```

---

## Build With Tag

```bash
docker build -t myapp:v1 .
```

---

## Build Without Cache

```bash
docker build --no-cache -t myapp .
```

---

## Build Specific Dockerfile

```bash
docker build -f Dockerfile.prod .
```

---

# Common Dockerfile Instructions

## FROM

Base image.

```dockerfile
FROM ubuntu:24.04
```

---

## WORKDIR

```dockerfile
WORKDIR /app
```

---

## COPY

```dockerfile
COPY . .
```

---

## ADD

Supports URLs and archives.

```dockerfile
ADD app.tar.gz .
```

---

## RUN

Executed during build.

```dockerfile
RUN apt-get update
```

---

## CMD

Default command.

```dockerfile
CMD ["java","-jar","app.jar"]
```

---

## ENTRYPOINT

Main executable.

```dockerfile
ENTRYPOINT ["java"]
```

---

## EXPOSE

Documentation for ports.

```dockerfile
EXPOSE 8080
```

---

## ENV

```dockerfile
ENV SPRING_PROFILES_ACTIVE=dev
```

---

# 7. Volumes

Without volumes:

```text
Container Deleted
    ↓
Data Lost
```

Volumes solve this.

---

## Create Volume

```bash
docker volume create pgdata
```

---

## List Volumes

```bash
docker volume ls
```

---

## Inspect

```bash
docker volume inspect pgdata
```

---

## Remove

```bash
docker volume rm pgdata
```

---

# Mount Volume

```bash
docker run \
-v pgdata:/var/lib/postgresql/data \
postgres
```

---

# Bind Mount

```bash
docker run \
-v $(pwd):/app \
node
```

Host folder mounted directly.

---

# Volume vs Bind Mount

| Volume         | Bind Mount    |
| -------------- | ------------- |
| Docker managed | Host managed  |
| Preferred      | Development   |
| Portable       | Less portable |

---

# 8. Networks

Containers communicate through networks.

---

## List Networks

```bash
docker network ls
```

---

## Create Network

```bash
docker network create app-net
```

---

## Inspect Network

```bash
docker network inspect app-net
```

---

## Remove Network

```bash
docker network rm app-net
```

---

## Run Container on Network

```bash
docker run \
--network app-net \
nginx
```

---

# Default Networks

## bridge

Default.

```text
Container ↔ Container
```

---

## host

Uses host network.

---

## none

No networking.

---

# 9. Environment Variables

Pass configuration.

```bash
docker run \
-e DB_HOST=postgres \
-e DB_PORT=5432 \
myapp
```

---

# Environment File

.env

```text
DB_HOST=localhost
DB_PORT=5432
```

Use:

```bash
docker run --env-file .env myapp
```

---

# 10. Port Mapping

Container:

```text
8080
```

Host:

```text
8081
```

Mapping:

```bash
docker run -p 8081:8080 myapp
```

Format:

```text
HOST:CONTAINER
```

---

# Multiple Ports

```bash
docker run \
-p 8080:8080 \
-p 9090:9090 \
myapp
```

---

# 11. Docker Compose

Manage multiple containers.

---

docker-compose.yml

```yaml
services:

  postgres:
    image: postgres:17

  app:
    image: myapp
```

---

## Start

```bash
docker compose up
```

Detached:

```bash
docker compose up -d
```

---

## Stop

```bash
docker compose down
```

---

## Rebuild

```bash
docker compose up --build
```

---

## Logs

```bash
docker compose logs
```

---

## Specific Service

```bash
docker compose logs app
```

---

# Compose Example

```yaml
services:

  postgres:
    image: postgres:17

  redis:
    image: redis:8

  app:
    build: .
    ports:
      - "8080:8080"
```

---

# 12. Registries

Store images.

---

## Login

```bash
docker login
```

---

## Tag Image

```bash
docker tag myapp myrepo/myapp:v1
```

---

## Push

```bash
docker push myrepo/myapp:v1
```

---

## Pull

```bash
docker pull myrepo/myapp:v1
```

---

# 13. Resource Management

---

## CPU Limit

```bash
docker run --cpus="2" myapp
```

---

## Memory Limit

```bash
docker run -m 1g myapp
```

---

## Memory + CPU

```bash
docker run \
-m 2g \
--cpus=2 \
myapp
```

---

# 14. Logs

---

## Show Logs

```bash
docker logs container
```

---

## Follow

```bash
docker logs -f container
```

---

## Tail

```bash
docker logs --tail=100 container
```

---

## Last Hour

```bash
docker logs --since=1h container
```

---

# 15. Debugging

---

## Open Shell

Linux:

```bash
docker exec -it container bash
```

or

```bash
docker exec -it container sh
```

---

## Process List

```bash
docker top container
```

---

## Resource Usage

```bash
docker stats
```

---

## Inspect

```bash
docker inspect container
```

---

## View Events

```bash
docker events
```

---

# 16. Security

---

## Run Non-Root

```dockerfile
RUN adduser appuser

USER appuser
```

---

## Scan Images

```bash
docker scout quickview image
```

---

## Use Official Images

Prefer:

```text
nginx
postgres
redis
eclipse-temurin
```

---

# 17. Cleanup Operations

---

## Remove Stopped Containers

```bash
docker container prune
```

---

## Remove Images

```bash
docker image prune
```

---

## Remove Volumes

```bash
docker volume prune
```

---

## Remove Networks

```bash
docker network prune
```

---

## Nuclear Cleanup

```bash
docker system prune -a
```

WARNING:

Removes almost everything unused.

---

# 18. Production Best Practices

---

Use specific tags:

Bad:

```text
postgres:latest
```

Good:

```text
postgres:17.4
```

---

Use Multi-stage Builds

```dockerfile
FROM maven AS build

FROM eclipse-temurin
```

---

Keep Images Small

Avoid:

```dockerfile
FROM ubuntu
```

When possible use:

```dockerfile
FROM alpine
```

---

Use Health Checks

```dockerfile
HEALTHCHECK CMD curl -f localhost:8080
```

---

Do Not Store Secrets

Bad:

```dockerfile
ENV PASSWORD=admin
```

---

# 19. Common Workflows

---

# Spring Boot Application

Build:

```bash
./mvnw clean package
```

Docker:

```dockerfile
FROM eclipse-temurin:21-jre

COPY target/*.jar app.jar

CMD ["java","-jar","app.jar"]
```

Build:

```bash
docker build -t app .
```

Run:

```bash
docker run -p 8080:8080 app
```

---

# PostgreSQL

```bash
docker run \
-d \
--name postgres \
-e POSTGRES_PASSWORD=admin \
-p 5432:5432 \
postgres:17
```

---

# Redis

```bash
docker run \
-d \
--name redis \
-p 6379:6379 \
redis
```

---

# Mount Current Directory

```bash
docker run \
-v $(pwd):/app \
ubuntu
```

---

# Copy File Into Container

```bash
docker cp app.jar container:/tmp/
```

---

# Copy File Out

```bash
docker cp container:/tmp/log.txt .
```

---

# 20. Troubleshooting

---

## Port Already Allocated

```text
Bind for 0.0.0.0:8080 failed
```

Find process:

```bash
lsof -i :8080
```

---

## Cannot Connect To Container

Check:

```bash
docker ps
```

---

## Container Exits Immediately

Inspect:

```bash
docker logs container
```

---

## Image Not Found

Run:

```bash
docker pull image
```

---

## Out Of Space

Check:

```bash
docker system df
```

Cleanup:

```bash
docker system prune -a
```

---

# 21. Command Cheat Sheet

## Images

```bash
docker images
docker pull IMAGE
docker build -t NAME .
docker rmi IMAGE
docker history IMAGE
```

---

## Containers

```bash
docker ps
docker ps -a
docker run IMAGE
docker stop ID
docker start ID
docker restart ID
docker rm ID
docker exec -it ID bash
```

---

## Volumes

```bash
docker volume create NAME
docker volume ls
docker volume inspect NAME
docker volume rm NAME
```

---

## Networks

```bash
docker network ls
docker network create NAME
docker network inspect NAME
docker network rm NAME
```

---

## Logs

```bash
docker logs ID
docker logs -f ID
docker logs --tail=100 ID
```

---

## Compose

```bash
docker compose up
docker compose up -d
docker compose down
docker compose logs
docker compose ps
```

---

## Cleanup

```bash
docker container prune
docker image prune
docker volume prune
docker network prune
docker system prune -a
```

---

# Golden Rule

Whenever you are confused, answer these four questions:

```text
1. What IMAGE am I using?
2. What CONTAINER is running?
3. What PORTS are exposed?
4. Where is the DATA stored?
```

95% of Docker debugging comes down to one of those four questions.
