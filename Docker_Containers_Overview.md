# 🐳 Docker and Containers Overview

## 📘 What is a Container?
A container is a lightweight, standalone, and executable software package that includes:
- Application code
- Runtime
- System tools & libraries
- Configuration files

Containers run consistently across environments and are isolated from the host system and each other.

## 🐳 What is Docker?
Docker is an open-source platform that enables developers to build, ship, and run containers easily and efficiently.
- Uses Docker Engine to create and manage containers.
- Simplifies packaging and distribution of applications.
- Provides tools to build container images, run containers, and orchestrate them.

## 🧱 Core Docker Components
| Component        | Description                                              |
|------------------|----------------------------------------------------------|
| Dockerfile       | Text file with instructions to build a Docker image      |
| Docker Image     | Read-only template used to create containers             |
| Docker Container | Running instance of a Docker image                       |
| Docker Engine    | Runtime that runs containers                             |
| Docker Hub       | Public cloud registry for Docker images                  |
| Volumes          | Used to persist and share data among containers          |
| Docker Compose   | Tool for defining and running multi-container applications|

## 🛠️ Docker Workflow
### 1. Create a Dockerfile
\`\`\`Dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
\`\`\`

### 2. Build an Image
\`\`\`bash
docker build -t my-node-app .
\`\`\`

### 3. Run a Container
\`\`\`bash
docker run -d -p 3000:3000 --name mycontainer my-node-app
\`\`\`

### 4. Push to Docker Hub
\`\`\`bash
docker tag my-node-app username/my-node-app
docker push username/my-node-app
\`\`\`

## 📦 Docker Images
- Built from Dockerfile instructions.
- Layered architecture — each Dockerfile command adds a layer.
- Cached layers optimize build time.
- Immutable — changes require new image builds.

## 🧪 Docker Containers
- Instance of an image.
- Has its own filesystem, network, and processes.
- Isolated but can share resources (volumes, networks).

### Common Commands
\`\`\`bash
docker ps
docker exec -it <id> sh
docker stop <id>
docker rm <id>
\`\`\`

## 🔗 Docker Volumes
\`\`\`bash
docker volume create myvol
docker run -v myvol:/data ubuntu
\`\`\`

## 🌐 Docker Networking
| Mode   | Description                                   |
|--------|-----------------------------------------------|
| bridge | Default network for standalone containers     |
| host   | Uses host's network stack                     |
| none   | Isolated with no network                      |
| overlay| Multi-host communication in Docker Swarm      |

\`\`\`bash
docker network create my-bridge
docker run --network=my-bridge nginx
\`\`\`

## 🧾 Docker Compose
\`\`\`yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "8080:80"
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: example
\`\`\`
Run with:
\`\`\`bash
docker-compose up
\`\`\`

## ✅ Docker Use Cases
- Microservices architecture
- CI/CD pipelines
- Application portability
- Isolated dev/test environments
- Edge or hybrid cloud deployments

## 🔐 Security Best Practices
- Use official images
- Regularly scan images
- Limit container privileges
- Don’t run as root inside containers
- Enable resource limits
- Use secrets management

## 📈 Monitoring Tools
| Tool              | Description                         |
|------------------|-------------------------------------|
| Docker stats      | CLI command to monitor containers   |
| cAdvisor          | Real-time container metrics         |
| Prometheus + Grafana| Advanced container monitoring     |
| Datadog / New Relic| Full observability platforms       |

## 🧠 Docker vs. Virtual Machines
| Feature        | Docker Container | Virtual Machine |
|----------------|------------------|------------------|
| Startup Time   | Seconds          | Minutes          |
| Size           | MBs              | GBs              |
| Performance    | Near-native      | Slight overhead  |
| Isolation      | Process-level    | OS-level         |
| OS Overhead    | Shares host OS   | Separate guest OS|

## 📚 Helpful Resources
- Docker Documentation
- Play with Docker (lab)
- Docker Hub
- Docker Cheat Sheet (PDF)

## 🔄 Docker and CI/CD Integration
- **GitHub Actions**: Build/test/push Docker images.
- **Jenkins**: Docker agents and isolated builds.
- **AWS CodeBuild + ECR**: Deploy containers to ECS.
- **GitLab CI**: Native Docker support.

## 🔧 Common Docker CLI Commands
| Command                         | Description              |
|--------------------------------|--------------------------|
| docker build -t name .         | Build image              |
| docker run -d -p 80:80 name    | Run container            |
| docker exec -it id sh          | Shell into container     |
| docker logs id                 | View logs                |
| docker push username/image     | Push to Docker Hub       |
| docker-compose up              | Start Compose services   |
| docker image prune             | Clean up images          |