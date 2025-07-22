
# 4️⃣ Advanced Docker Topics & Labs

## Q10: Docker Swarm vs. Kubernetes

| Feature                | Docker Swarm                          | Kubernetes                          |
|-----------------------|---------------------------------------|-------------------------------------|
| **Complexity**        | Simple, built into Docker             | More complex, steeper learning curve |
| **Setup**            | Quick to set up (docker swarm init)   | Requires more configuration         |
| **Scaling**          | Manual scaling (docker service scale) | Auto-scaling available              |
| **Networking**       | Simpler overlay networks              | Advanced CNI plugins                |
| **Best for**         | Small to medium clusters              | Large, complex deployments          |

**Lab**: Initialize a Swarm cluster
```bash
docker swarm init
docker service create --name web --replicas 3 -p 80:80 nginx
docker service ls
```

## Q11: Secure a Docker Container (Best Practices)

**Security Checklist**:
1. **Non-root user**:
   ```dockerfile
   FROM alpine
   RUN adduser -D myuser
   USER myuser
   ```

2. **Scan images**:
   ```bash
   docker scan <image-name>
   ```

3. **Runtime protection**:
   ```bash
   docker run --read-only --security-opt no-new-privileges my_image
   ```

4. **Network hardening**:
   ```bash
   docker network create --internal private_net
   ```

**Lab**: Create a secured container
```bash
docker run -d --name secured \
  --user 1000 \
  --read-only \
  --security-opt no-new-privileges \
  --cap-drop ALL \
  nginx:alpine
```

## Q12: Multi-stage Builds — Why and How?

**Why**:
- Reduce final image size
- Remove build dependencies
- Keep build tools separate

**Example** (Go application):
```dockerfile
# Build stage
FROM golang:1.19 as builder
WORKDIR /app
COPY . .
RUN go build -o myapp

# Final stage
FROM alpine:latest
WORKDIR /root/
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

**Lab**: Build and compare sizes
```bash
docker build -t single-stage .
docker build -t multi-stage -f Dockerfile.multi .
docker images | grep -E 'single-stage|multi-stage'
```

## Additional Advanced Questions

### Q13: Implement CI/CD with Docker
**Solution**:
```yaml
# .gitlab-ci.yml example
stages:
  - build
  - deploy

build_image:
  stage: build
  script:
    - docker build -t myapp:$CI_COMMIT_SHA .
    - docker push myapp:$CI_COMMIT_SHA

deploy_prod:
  stage: deploy
  script:
    - docker stack deploy -c docker-compose.prod.yml myapp
```

### Q14: Optimize Docker Build Cache
**Techniques**:
1. Order Dockerfile commands from least to most frequent changes
2. Use explicit versions for base images
3. Combine related commands:
   ```dockerfile
   RUN apt-get update && \
       apt-get install -y package && \
       rm -rf /var/lib/apt/lists/*
   ```

### Q15: Handle Persistent Data in Swarm
**Solution**:
```yaml
version: '3.8'

services:
  db:
    image: postgres
    volumes:
      - db-data:/var/lib/postgresql/data
    deploy:
      placement:
        constraints: [node.role == manager]

volumes:
  db-data:
    driver: local
```

## Hands-on Labs
1. **Swarm Visualizer**:
   ```bash
   docker service create \
     --name=viz \
     --publish=8080:8080/tcp \
     --constraint=node.role==manager \
     --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
     dockersamples/visualizer
   ```

2. **Multi-container App**:
   ```bash
   git clone https://github.com/docker/awesome-compose
   cd awesome-compose/nginx-flask-mysql
   docker-compose up -d
   ```

[⬆ Back to Main Questions](../README.md) | [🛠 Try More Scenarios](../scenarios/)
```
