# 3️⃣ Real-World Docker Scenarios

## Q7: How would you reduce a Docker image size by 80%?

**Solution Approach:**
1. **Start with appropriate base image**:
   ```dockerfile
   FROM python:3.9-slim-buster  # (~100MB) vs python:3.9 (~900MB)
   ```

2. **Multi-stage builds**:
   ```dockerfile
   # Builder stage
   FROM node:16 as builder
   WORKDIR /app
   COPY . .
   RUN npm install && npm run build

   # Final stage
   FROM nginx:alpine
   COPY --from=builder /app/dist /usr/share/nginx/html
   ```

3. **Cleanup in same layer**:
   ```dockerfile
   RUN apt-get update && \
       apt-get install -y --no-install-recommends build-essential && \
       # Install dependencies... && \
       apt-get remove -y build-essential && \
       apt-get autoremove -y && \
       rm -rf /var/lib/apt/lists/*
   ```

4. **Other optimizations**:
   - Use `.dockerignore` to exclude unnecessary files
   - Combine related commands to minimize layers
   - Remove cached package manager files

**Expected Results**: Typical image size reduction from 1GB → 200MB

---

## Q8: Deploy a Flask app with Redis using docker-compose

**Solution** (`docker-compose.yml`):
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      - REDIS_HOST=redis
    depends_on:
      - redis

  redis:
    image: redis:alpine
    volumes:
      - redis_data:/data

volumes:
  redis_data:
```

**Flask App Dockerfile**:
```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
```

**Key Components**:
1. Dedicated network created automatically
2. Persistent storage for Redis
3. Explicit service dependencies
4. Environment variables for service discovery

---

## Q9: Debug a network issue between two containers

**Troubleshooting Steps**:

1. **Verify basic connectivity**:
   ```bash
   docker exec -it container1 ping container2
   ```

2. **Check network configuration**:
   ```bash
   docker network inspect bridge
   docker inspect container1 | grep IPAddress
   ```

3. **Test port accessibility**:
   ```bash
   docker exec container1 curl -v http://container2:3000
   ```

4. **Common fixes**:
   - Ensure containers are on same network:
     ```bash
     docker network create mynet
     docker run --network=mynet ...
     ```
   - Verify exposed ports in Dockerfile
   - Check application logs:
     ```bash
     docker logs container2
     ```
   - Test with simplified images (`busybox`) to isolate the issue

5. **Advanced tools**:
   ```bash
   docker exec -it container1 tcpdump -i eth0
   docker exec -it container1 netstat -tulnp
   ```

---

[⬆ Back to Main Questions](../README.md) | [🛠 Try Hands-on Lab](../docker-compose-lab/)
```
