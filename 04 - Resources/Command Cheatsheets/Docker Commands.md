---
type: cheatsheet
tags: [commands, docker, containers, cheatsheet]
created: 2026-03-02
---

# Docker Commands

---

## Container Lifecycle

```bash
docker run -d --name myapp -p 8080:80 nginx
```

```bash
docker run -it --rm ubuntu bash
```
> Run interactive container, remove on exit

```bash
docker run -d --name myapp -v /host/path:/container/path -e ENV_VAR=value image:tag
```

```bash
docker run -d --restart=always --name myapp image:tag
```

```bash
docker start container-name
```

```bash
docker stop container-name
```

```bash
docker restart container-name
```

```bash
docker kill container-name
```

```bash
docker rm container-name
```

```bash
docker rm -f container-name
```
> Force remove running container

```bash
docker rm $(docker ps -aq)
```
> Remove all stopped containers

---

## Container Interaction

```bash
docker exec -it container-name bash
```

```bash
docker exec -it container-name sh
```

```bash
docker exec container-name command
```

```bash
docker logs container-name
```

```bash
docker logs -f container-name
```
> Follow logs in real-time

```bash
docker logs --tail 100 container-name
```

```bash
docker logs --since 1h container-name
```

```bash
docker inspect container-name
```

```bash
docker stats
```
> Live resource usage for all containers

```bash
docker top container-name
```

```bash
docker cp container-name:/path/file ./local-path
```

```bash
docker cp ./local-file container-name:/path/
```

---

## Container Listing

```bash
docker ps
```
> Running containers

```bash
docker ps -a
```
> All containers (including stopped)

```bash
docker ps -q
```
> Only container IDs

```bash
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

---

## Images

```bash
docker images
```

```bash
docker build -t myimage:tag .
```

```bash
docker build -t myimage:tag -f Dockerfile.dev .
```

```bash
docker build --no-cache -t myimage:tag .
```

```bash
docker pull image:tag
```

```bash
docker push user/image:tag
```

```bash
docker tag image:old user/image:new
```

```bash
docker rmi image:tag
```

```bash
docker rmi $(docker images -q --filter "dangling=true")
```
> Remove all dangling images

```bash
docker image prune -a
```
> Remove all unused images

```bash
docker save -o image.tar image:tag
```

```bash
docker load -i image.tar
```

```bash
docker history image:tag
```

---

## Networks

```bash
docker network ls
```

```bash
docker network create mynetwork
```

```bash
docker network create --driver bridge mynetwork
```

```bash
docker network connect mynetwork container-name
```

```bash
docker network disconnect mynetwork container-name
```

```bash
docker network inspect mynetwork
```

```bash
docker network rm mynetwork
```

---

## Volumes

```bash
docker volume ls
```

```bash
docker volume create myvolume
```

```bash
docker volume inspect myvolume
```

```bash
docker volume rm myvolume
```

```bash
docker volume prune
```
> Remove all unused volumes

---

## Docker Compose

```bash
docker compose up -d
```

```bash
docker compose up -d --build
```
> Build images then start

```bash
docker compose down
```

```bash
docker compose down -v
```
> Stop and remove volumes

```bash
docker compose build
```

```bash
docker compose build --no-cache
```

```bash
docker compose logs -f
```

```bash
docker compose logs -f service-name
```

```bash
docker compose exec service-name bash
```

```bash
docker compose ps
```

```bash
docker compose pull
```

```bash
docker compose restart service-name
```

```bash
docker compose stop
```

```bash
docker compose config
```
> Validate and view composed config

```bash
docker compose scale service=3
```

---

## System & Cleanup

```bash
docker system df
```
> Show disk usage

```bash
docker system prune
```
> Remove stopped containers, unused networks, dangling images

```bash
docker system prune -a --volumes
```
> Remove EVERYTHING unused (DANGEROUS)

```bash
docker info
```

```bash
docker version
```

---

## Dockerfile Reference

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
ENV APP_ENV=production
CMD ["python", "app.py"]
```

### Multi-stage Build
```dockerfile
FROM node:20 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
```

---

*See also: [[Kubernetes Commands]] | [[Nginx Commands]]*
