# 🐳 Docker Command Cheat Sheet (Developer Friendly)

---

## 🔹 Docker Basics

### Check Docker installation

```bash
docker --version
docker info
```

### List Docker resources

```bash
docker ps            # running containers
docker ps -a         # all containers
docker images        # local images
docker volume ls     # volumes
docker network ls    # networks
```

---

## 🔹 Images

### Build an image

```bash
docker build -t my-app .
```

### Build without cache (debugging)

```bash
docker build --no-cache -t my-app .
```

### List images

```bash
docker images
```

### Remove image

```bash
docker rmi image-name
```

### Remove dangling images

```bash
docker image prune
```

---

## 🔹 Containers

### Run a container

```bash
docker run my-app
```

### Run in detached mode + port mapping

```bash
docker run -d -p 4200:4200 my-app
```

### Name a container

```bash
docker run --name my-container my-app
```

### Stop a container

```bash
docker stop my-container
```

### Remove a container

```bash
docker rm my-container
```

### Stop + remove automatically

```bash
docker run --rm my-app
```

---

## 🔹 Logs & Debugging

### View logs

```bash
docker logs my-container
```

### Follow logs

```bash
docker logs -f my-container
```

### Exec into running container

```bash
docker exec -it my-container sh
```

> Use `bash` instead of `sh` if available.

---

## 🔹 Ports & Networking (Very Important)

### Publish ports

```bash
-p 4200:4200
```

### Common meanings

| Inside Docker        | Means                    |
| -------------------- | ------------------------ |
| localhost            | same container           |
| 0.0.0.0              | listen on all interfaces |
| service-name         | another container        |
| host.docker.internal | host machine             |

---

## 🔹 Volumes

### Mount local folder

```bash
docker run -v $(pwd):/app my-app
```

### Named volume

```bash
docker volume create my-volume
docker run -v my-volume:/data my-app
```

### Remove unused volumes

```bash
docker volume prune
```

---

## 🔹 Docker Compose (Daily Use)

### Start services

```bash
docker compose up
```

### Start in background

```bash
docker compose up -d
```

### Stop services

```bash
docker compose down
```

### Rebuild images

```bash
docker compose up --build
```

### View running services

```bash
docker compose ps
```

### View logs

```bash
docker compose logs -f
```

---

## 🔹 Cleaning Up (Use Carefully)

### Remove stopped containers

```bash
docker container prune
```

### Remove everything unused

```bash
docker system prune
```

> ⚠️ This deletes stopped containers, unused images, networks.

---

## 🔹 Dockerfile Keywords (Quick Reference)

```dockerfile
FROM        # base image
WORKDIR     # working directory
COPY        # copy files
RUN         # execute during build
CMD         # default command
ENTRYPOINT  # fixed command
EXPOSE      # documentation only
ENV         # environment variables
```

---

## 🔹 Debug Checklist (Save this!)

When something doesn’t work:

1. Is container running?

```bash
docker ps
```

2. Are ports exposed?

```bash
docker port container-name
```

3. Is app listening on `0.0.0.0`?

```bash
netstat -tulpn
```

4. Logs?

```bash
docker logs container-name
```

5. Rebuild required?

```bash
docker build .
```

---

## 🔹 Common Mistakes (You already hit these 😄)

❌ Using `localhost` incorrectly
❌ Forgetting `--host 0.0.0.0`
❌ Not rebuilding images
❌ Expecting EXPOSE to publish ports
❌ Mixing host & container assumptions

---

## 🔹 Pro Tips

* Prefer **Docker Compose** for multi-service apps
* One container = one responsibility
* Use service names instead of IPs
* Don’t fight Docker networking — understand it

---

## 🧭 What This Cheat Sheet Prepares You For

* Angular + Docker
* Spring Boot + Docker
* PostgreSQL containers
* JHipster
* Microservices

