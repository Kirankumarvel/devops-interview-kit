# 🐳 Docker Compose Hands-on Labs

## Lab 1: Basic Web Stack (Nginx + Node.js + Redis)

1. Create `docker-compose.yml`:
```yaml
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - app

  app:
    build: ./node-app
    environment:
      - REDIS_HOST=redis
    restart: unless-stopped

  redis:
    image: redis:alpine
    volumes:
      - redis_data:/data

volumes:
  redis_data:
```

2. Create `node-app/Dockerfile`:
```dockerfile
FROM node:16-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["node", "server.js"]
```

3. Start the stack:
```bash
docker-compose up -d --build
```
👉 Access at http://localhost:8080

---

## Lab 2: Multi-container WordPress

```yaml
version: '3.8'

services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass

  wordpress:
    image: wordpress:php7.4-apache
    ports:
      - "8000:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass
    depends_on:
      - db

volumes:
  db_data:
```

👉 Access WordPress setup at http://localhost:8000

---

## Lab 3: CI/CD Pipeline with Compose

1. `docker-compose.ci.yml`:
```yaml
version: '3.8'

services:
  test:
    build: .
    command: ["npm", "test"]
    volumes:
      - ./:/app

  build:
    image: docker:stable
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    command: >
      sh -c "docker build -t myapp:latest . &&
             docker push myapp:latest"
```

2. Run tests and build:
```bash
docker-compose -f docker-compose.ci.yml up test
docker-compose -f docker-compose.ci.yml up build
```

---

## Lab 4: Monitoring Stack (Prometheus + Grafana)

```yaml
version: '3.8'

services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    volumes:
      - grafana_data:/var/lib/grafana
    depends_on:
      - prometheus

volumes:
  grafana_data:
```

👉 Access:
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000 (admin/admin)

---

## Lab 5: Debugging Network Issues

1. Create isolated network:
```bash
docker network create --driver bridge --subnet 192.168.100.0/24 debug-net
```

2. `docker-compose.debug.yml`:
```yaml
version: '3.8'

services:
  service1:
    image: alpine
    command: ["ping", "service2"]
    networks:
      debug-net:
        ipv4_address: 192.168.100.10

  service2:
    image: alpine
    command: ["tail", "-f", "/dev/null"]
    networks:
      debug-net:
        ipv4_address: 192.168.100.20

networks:
  debug-net:
    external: true
```

3. Debug commands:
```bash
docker-compose -f docker-compose.debug.yml up
docker exec -it debug_service1_1 ping 192.168.100.20
docker exec -it debug_service2_1 ifconfig
```

---

## 🎯 Lab Challenges

1. **Load Balancing**: Add Traefik to Lab 1
2. **Secret Management**: Store MySQL credentials securely
3. **Zero-Downtime Deploy**: Update Node.js app without downtime
4. **Resource Limits**: Enforce CPU/memory limits
5. **Health Checks**: Add to all services

[📚 Solution Examples](./solutions/) | [⬆ Back to Main](../README.md)
```
