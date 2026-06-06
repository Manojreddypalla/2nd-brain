# Docker Cheat Sheet – Complete Revision

# 

---

## **1. Application Containerization**

- **Concept:** Package apps with all dependencies into a container for consistency across environments.
- **Key Benefits:** Portability, isolation, dev/prod parity.
- **Command Examples:**

```bash
docker run hello-world      # Test Docker installation
docker ps -a                # List all containers
docker stop <container>     # Stop a running container
docker rm <container>       # Remove a container

```

---

## **2. Image Creation (from Dockerfiles)**

- **Dockerfile Basics:**

```docker
FROM ubuntu:22.04         # Base image
RUN apt-get update && apt-get install -y python3
COPY . /app                # Copy local files
WORKDIR /app               # Set working directory
RUN pip install -r requirements.txt
CMD ["python3", "app.py"] # Default command
EXPOSE 5000               # Expose port

```

- **Build an image:**

```bash
docker build -t myapp:latest .
docker images             # List images

```

- **Tag image for registry:**

```bash
docker tag myapp:latest username/myapp:latest

```

---

## **3. Container Execution (Running Images)**

- **Basic run:**

```bash
docker run -it myapp       # Interactive container
docker run -d myapp        # Detached container
docker run -p 5000:5000 myapp  # Map ports

```

- **Check logs & exec:**

```bash
docker logs <container>
docker exec -it <container> bash  # Enter running container

```

- **Stop and remove containers:**

```bash
docker stop <container>
docker rm <container>

```

---

## **4. Image Registry Management (Docker Hub)**

- **Login:**

```bash
docker login

```

- **Push an image:**

```bash
docker push username/myapp:latest

```

- **Pull an image:**

```bash
docker pull username/myapp:latest

```

- **Search images:**

```bash
docker search nginx

```

---

## **5. Portability (Dev/Prod Parity)**

- Containers behave the same across dev, staging, and production.
- Use **environment variables**:

```bash
docker run -e ENV=production myapp

```

---

## **6. Dependency Isolation & Management**

- Each container has its own filesystem and dependencies.
- Prevents conflicts between app versions.
- **Example:** Python app vs Node app on the same host.

---

## **7. Persistent Data Management (Volumes)**

- **Create a volume:**

```bash
docker volume create mydata

```

- **Mount a volume to container:**

```bash
docker run -v mydata:/app/data myapp

```

- **Bind mount (host directory):**

```bash
docker run -v /host/path:/container/path myapp

```

---

## **8. Container Networking**

- **Default bridge network:** isolates containers.
- **List networks:**

```bash
docker network ls

```

- **Create custom network:**

```bash
docker network create mynet
docker run --network=mynet --name app1 myapp
docker run --network=mynet --name app2 myapp

```

- Containers on the same network can communicate via container name.

---

## **9. Multi-Container Orchestration (Docker Compose)**

- **docker-compose.yml Example:**

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/app
    networks:
      - mynet
  redis:
    image: redis
    networks:
      - mynet

networks:
  mynet:
    driver: bridge

```

- **Commands:**

```bash
docker-compose up -d     # Start all services
docker-compose down      # Stop and remove services
docker-compose logs -f   # View logs

```

---

## **10. Cluster Orchestration (Docker Swarm)**

- **Initialize swarm:**

```bash
docker swarm init

```

- **Deploy stack:**

```bash
docker stack deploy -c docker-compose.yml mystack

```

- **List services:**

```bash
docker service ls
docker node ls

```

---

## **11. Secret & Configuration Management**

- **Create secret:**

```bash
echo "mypassword" | docker secret create db_password -

```

- **Use in service:**

```yaml
services:
  db:
    image: postgres
    secrets:
      - db_password

secrets:
  db_password:
    external: true

```

- **List secrets:** `docker secret ls`
- **Remove secret:** `docker secret rm <name>`

---

## **12. Docker MCP Toolkit (MCP Servers & Gateways)**

- **Install MCP CLI plugin:**

```bash
git clone https://github.com/docker/mcp-gateway.git
cd mcp-gateway
make docker-mcp
cp dist/docker-mcp ~/.docker/cli-plugins/
chmod +x ~/.docker/cli-plugins/docker-mcp

```

- **List MCP servers:**

```bash
docker mcp server list

```

- **Enable a server:**

```bash
docker mcp server enable notion

```

- **Run MCP gateway:**

```bash
docker mcp gateway run

```

- **Set secret (API key for a server):**

```bash
docker mcp secret set notion.NOTION_API_KEY="your-token"

```

- **Edit catalog & registry files:**

```bash
nano ~/.docker/mcp/catalogs/docker-mcp.yaml
nano ~/.docker/mcp/registry.yaml

```

---

## **13. Useful Commands & Tips**

```bash
docker ps -a                    # List all containers
docker images                   # List all images
docker system prune -a          # Cleanup unused images/containers
docker inspect <container>      # Inspect container details
docker stats                     # Real-time container stats
docker info                      # Docker system info

```

**Tip:** Use **project-specific folders** for MCP YAMLs to avoid conflicts. Always check **ports & IPs** for networked MCP servers.