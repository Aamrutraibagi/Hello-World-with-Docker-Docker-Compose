# 🐳 Hello World with Docker & Docker Compose

This project demonstrates how to use Docker and Docker Compose to run a server-client setup. Along the way, you’ll also learn some basic Docker concepts like images, containers, port mapping, and networking.
---

#🔹 What is Docker?

Docker is a platform that allows you to:
```
Package applications and dependencies into containers.

Run containers anywhere (your laptop, server, or cloud) in the same way.

Avoid issues like “works on my machine”.

Think of a container as a lightweight, isolated environment that contains everything your app needs (code, libraries, OS dependencies).
```
---
# 🔹 Why Use Docker?

✅ Consistency: Same environment across dev, test, prod.

✅ Portability: Run anywhere without worrying about setup.

✅ Isolation: Each app runs in its own container.

✅ Easy scaling: Run multiple instances with docker-compose or Kubernetes.

# 🔹 Basic Docker Concepts

## 1️⃣ Images & Containers

Image: Blueprint (recipe) for creating containers.

Container: A running instance of an image.

👉 Example:
```
# Build an image
docker build -f Dockerfile.txt -t hello-world-2 .

# Run a container from the image
docker run hello-world-2:latest
```
Output:
```
Hello World from Docker
```

## 2️⃣ Port Mapping (-p)

Containers run in isolation. To access a service inside, we map ports from container → host.

👉 Example:
```
docker run -d -p 8000:8080 hello-world-3
```

8080 → server’s port inside container

8000 → port exposed on your machine

So now, hitting http://localhost:8000 reaches the container’s port 8080.
---

## 3️⃣ Networking (--network)

By default, containers can’t talk to each other unless on the same Docker network.

👉 Example:
```
# Create a custom network
docker network create mynetwork

# Run server in the network
docker run -d --network mynetwork --name server hello-world-3

# Run client in the same network
docker run --rm --network mynetwork curlimages/curl \
   curl -X POST server:8080 -H "Content-Type: application/json" -d "{\"username\":\"test\"}"
```

Here, the client can access the server by its container name (server) instead of IP.
---

4️⃣ Docker Compose

docker-compose.yml allows defining multiple containers and networks in a single file.

👉 Example:
```
# Build and start containers
docker-compose up --build
```

Both server and client start automatically and communicate via a shared network.
---

# 📂 Project Structure
```
hello-world-docker/
│── hello-world-3/
│   └── me_docker_qa/
│       ├── client.py
│       ├── server.py
│       ├── Dockerfile-client
│       ├── Dockerfile-server
│       ├── docker-compose.yml
│── Dockerfile.txt   # Single-container demo
```

🚀 Running the Project
1. Simple Hello World with Dockerfile
```
docker build -f Dockerfile.txt -t hello-world-2 .
docker run hello-world-2:latest
```

Output:
```
Hello World from Docker
```
---
2. Run Server Only
```
docker build -f Dockerfile-server -t hello-world-3 .
docker run -d -p 8000:8080 hello-world-3
```

Test:
```
curl -X POST -H "Content-Type: application/json" -d "{\"username\": \"yourname\"}" http://localhost:8000

```
Output:
```
Hello yourname
```
---
3. Run Server & Client with Docker Network

docker network create mynetwork
```
docker run -d --network mynetwork --name server hello-world-3

docker run --rm --network mynetwork curlimages/curl \
   curl -X POST server:8080 -H "Content-Type: application/json" -d "{\"username\":\"test\"}"

```
Output:
```
Hello test
```
---
4. Run with Docker Compose

```
docker-compose build
docker-compose up
```
Logs:
```
server-1  | POST /
client-1  | Hello John Doe
```

⚙️ Example docker-compose.yml
```
services:
  server:
    build:
      context: .
      dockerfile: Dockerfile-server
    ports:
      - "8000:8080"
    networks:
      - mynetwork

  client:
    build:
      context: .
      dockerfile: Dockerfile-client
    depends_on:
      - server
    networks:
      - mynetwork

networks:
  mynetwork:
```


# 🐳 Useful Docker Commands

```
# List running containers
docker ps

# Stop a container
docker stop <container_id>

# Remove a container
docker rm <container_id>

# List networks
docker network ls

# Remove a network
docker network rm mynetwork

# Clean up unused stuff
docker system prune -a
```

# ✅ With this project, you now know:

What Docker is & why it’s used

How to build images & run containers

How port mapping works (-p)

How networking works (--network)

How to orchestrate with Docker Compose
