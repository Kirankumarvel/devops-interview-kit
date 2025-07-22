
# 2️⃣ Docker Commands & Troubleshooting Cheatsheet

## 🐳 Essential Commands

### Container Management
```bash
# List running containers
docker ps

# List all containers (running + stopped)
docker ps -a

# Run container in detached mode
docker run -d --name my_container image:tag

# Stop/start container
docker stop my_container
docker start my_container

# Remove stopped container
docker rm my_container

# Remove container forcefully (running)
docker rm -f my_container
```

### Image Management
```bash
# List images
docker images

# Pull image from registry
docker pull image:tag

# Remove image
docker rmi image:tag

# Build image from Dockerfile
docker build -t my_image:version .
```

### Networking
```bash
# List networks
docker network ls

# Inspect container IP
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my_container
```

## 🔧 Troubleshooting

### Q5: Fix "Port already in use" error
**Solution 1:** Free the port
```bash
# Find process using the port
sudo lsof -i :8080

# Kill the process
kill -9 <PID>
```

**Solution 2:** Change container port mapping
```bash
docker run -p 8081:80 my_image  # Map host 8081 to container 80
```

**Solution 3:** Stop conflicting container
```bash
docker stop $(docker ps -q --filter ancestor=image_name)
```

### Q6: Debug a crashing container
**Step-by-step debugging:**
1. Check logs:
   ```bash
   docker logs my_container
   docker logs --tail 50 --follow my_container  # Tail last 50 lines
   ```

2. Inspect container metadata:
   ```bash
   docker inspect my_container
   ```

3. Enter stopped container:
   ```bash
   docker run -it --entrypoint sh image:tag
   ```

4. Check resource usage:
   ```bash
   docker stats
   ```

5. Verify health checks:
   ```bash
   docker events --filter 'event=health_status'
   ```

## 🚀 Advanced Commands
```bash
# Prune unused objects
docker system prune

# View resource usage
docker system df

# Copy files between host/container
docker cp my_container:/path/to/file /host/path
docker cp /host/path my_container:/container/path

# Save/load images as tar files
docker save -o my_image.tar image:tag
docker load -i my_image.tar
```

## 🆘 Help Resources
```bash
# Get command help
docker --help
docker <command> --help

# Check Docker version and info
docker version
docker info
```

[📚 Back to Main Questions](../README.md) | [💡 Practice Scenarios](./scenarios/)
```
