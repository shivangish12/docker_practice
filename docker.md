
# 📦 What is Docker?

**Docker** is a containerization platform used to build, package, and run applications in isolated environments called **containers**.

> It ensures the app runs the same across development, testing, and production.

---

# ❓ Why Docker?

Before Docker:

* Works on developer machine ❌ fails in production
* Dependency mismatch
* Environment configuration issues
* Hard to scale

Docker solves:
✔ Environment consistency
✔ Fast deployment
✔ Lightweight virtualization
✔ Easy scaling
✔ CI/CD friendly

---

# 🧱 Virtual Machines vs Containers

| Feature     | Virtual Machine | Docker Container |
| ----------- | --------------- | ---------------- |
| OS          | Full OS         | Shares Host OS   |
| Size        | GBs             | MBs              |
| Startup     | Minutes         | Seconds          |
| Performance | Slower          | Near Native      |
| Use Case    | Legacy apps     | Microservices    |

---

# 📦 Docker Architecture

Docker uses **Client-Server Architecture**:

```
Docker Client → Docker Daemon → Containers
```

## Components

### Docker Client

CLI used to run commands:

```
docker run
docker build
docker push
```

### Docker Daemon (`dockerd`)

Handles:

* Building images
* Running containers
* Managing networks & volumes

### Docker Registry

Stores images (Docker Hub, ECR, etc.)

---

# 🧩 Docker Objects

## 1️⃣ Images

Blueprint for containers (Read-only template).

```
docker build -t myapp .
```

## 2️⃣ Containers

Running instance of an image.

```
docker run myapp
```

## 3️⃣ Volumes

Persistent storage.

## 4️⃣ Networks

Communication layer between containers.

---

# 📝 Dockerfile

Script to build an image.

Example:

```
FROM node:20
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "start"]
```

---

# 🔄 Docker Lifecycle

```
Code → Build → Image → Run → Container → Push → Deploy
```

---

# ⚙️ Core Commands

## Build Image

```
docker build -t myapp .
```

## Run Container

```
docker run -d -p 3000:3000 myapp
```

## List Containers

```
docker ps
```

## Stop Container

```
docker stop <id>
```

## Remove Container

```
docker rm <id>
```

## View Images

```
docker images
```

---

# 📦 Docker Compose

Used to run **multi-container apps**.

Example:

```
services:
  app:
    build: .
    ports:
      - "3000:3000"

  db:
    image: mysql
```

Run:

```
docker compose up
```

---

# 🔨 docker build vs docker run vs docker compose

| Command        | Purpose            |
| -------------- | ------------------ |
| docker build   | Create image       |
| docker run     | Start container    |
| docker compose | Run full app stack |

---

# 📤 docker push (Publishing Image)

```
docker tag myapp username/myapp
docker push username/myapp
```

Used to deploy to:

* Docker Hub
* AWS ECR
* Kubernetes clusters

---

# 🔍 Port Mapping

```
docker run -p HOST:CONTAINER image
```

Example:

```
docker run -p 8080:80 nginx
```

Host → 8080
Container → 80

---

# 🌎 Environment Variables

```
docker run -e ENV=value image
```

Using `.env`:

```
docker run --env-file .env image
```

---

# 💾 Volumes (Persistent Data)

```
docker run -v myvolume:/data image
```

Prevents data loss when container stops.

---

# 🔗 Docker Networking

Default network = bridge.

Create custom network:

```
docker network create mynetwork
```

Run container inside:

```
docker run --network mynetwork image
```

Benefits:
✔ DNS-based communication
✔ Better isolation

---

# 📊 Resource Limits

Limit CPU & memory:

```
docker run --memory="512m" --cpus="1" image
```

Monitor usage:

```
docker stats
```

---

# 🏗️ Multi-Stage Builds

Used to reduce image size by separating build & runtime.

Example:

```
FROM node:20 AS builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

Benefits:
✔ Smaller image
✔ More secure
✔ Faster deploy

---

# 🧠 CMD vs ENTRYPOINT

| CMD               | ENTRYPOINT       |
| ----------------- | ---------------- |
| Default arguments | Fixed executable |
| Can override      | Hard to override |

Example:

```
ENTRYPOINT ["python"]
CMD ["app.py"]
```

---

# 📦 Container Defaults Override

Override runtime behavior:

```
docker run -p 8081:80 image
docker run -e ENV=prod image
docker run --memory="1g" image
```

No rebuild required.

---

# 🧪 Useful Debug Commands

View logs:

```
docker logs <container>
```

Enter container:

```
docker exec -it <container> bash
```

Inspect container:

```
docker inspect <id>
```

---

# 🧹 Cleanup Commands

Stop all:

```
docker stop $(docker ps -q)
```

Remove unused:

```
docker system prune
```

---

# 🚀 Real-World Workflow

## Development

```
docker compose up
```

## Build Release

```
docker build -t app:v1 .
```

## Push to Registry

```
docker push username/app:v1
```

## Deploy Anywhere

```
docker run app:v1
```

---


