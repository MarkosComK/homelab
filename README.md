# Home Server Setup Guide with Colima and Docker

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Container Strategy](#container-strategy)
- [Security and Permissions](#security-and-permissions)
- [Service Configuration](#service-configuration)
- [Deployment](#deployment)
- [Maintenance](#maintenance)

## Prerequisites

Before starting, ensure you have:
- macOS operating system
- Homebrew package manager installed
- At least 8GB RAM (16GB recommended)
- At least 256GB storage
- Stable internet connection

## Installation

### 1. Install Required Tools

```bash
# Install Colima
brew install colima

# Install Docker and Docker Compose
brew install docker docker-compose
```

### 2. Configure Colima

```bash
# Start Colima with custom resources (adjust based on your machine)
colima start --cpu 4 --memory 8 --disk 100

# Verify Docker installation
docker ps
```

## Container Strategy

### Multiple Containers vs Single Container

We'll use multiple containers for several reasons:
1. **Isolation**: Each service runs in its own environment
2. **Security**: If one service is compromised, others remain safe
3. **Resource Management**: Better control over resource allocation
4. **Scalability**: Easier to scale individual services
5. **Maintenance**: Can update/restart services independently

### Container Structure

```plaintext
my-homeserver/
├── docker-compose.yml
├── nginx/
│   ├── Dockerfile
│   └── conf/
├── website/
│   └── public/
├── game-server/
│   ├── Dockerfile
│   └── cube3d/
└── database/
    └── data/
```

### Example docker-compose.yml

```yaml
version: '3.8'

services:
  nginx:
    build: ./nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./website:/usr/share/nginx/html
    networks:
      - frontend
    restart: unless-stopped

  game-server:
    build: ./game-server
    ports:
      - "3000:3000"
    volumes:
      - ./game-server/cube3d:/app
    networks:
      - frontend
      - backend
    depends_on:
      - database
    restart: unless-stopped

  database:
    image: postgres:14
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    volumes:
      - ./database/data:/var/lib/postgresql/data
    networks:
      - backend
    restart: unless-stopped

networks:
  frontend:
  backend:

secrets:
  db_password:
    file: ./secrets/db_password.txt
```

## Security and Permissions

### Container Users

Each service should run as a non-root user:

```dockerfile
# Example Dockerfile for game-server
FROM node:16-alpine

# Create app directory and user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app

# Copy application files
COPY --chown=appuser:appgroup . .

# Switch to non-root user
USER appuser

CMD ["node", "server.js"]
```

### File Permissions

```bash
# Set proper permissions for mounted volumes
chmod -R 755 website/
chmod -R 750 database/data/
```

### Network Security

- Use separate networks for frontend and backend services
- Expose only necessary ports
- Use SSL/TLS for all external connections

## Service Configuration

### Website Configuration

1. Create Nginx configuration:
```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    location / {
        root /usr/share/nginx/html;
        index index.html;
    }

    location /game {
        proxy_pass http://game-server:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
    }
}
```

### Game Server Configuration

1. Set up the game server environment:
```javascript
// server.js example
const express = require('express');
const app = express();

app.use(express.static('public'));
app.listen(3000);
```

## Deployment

### Initial Deployment

```bash
# Build and start all services
docker-compose up -d

# Check service status
docker-compose ps

# View logs
docker-compose logs -f
```

### Backup Strategy

1. Create a backup script:
```bash
#!/bin/bash
# backup.sh
DATE=$(date +%Y%m%d)
docker-compose down
tar -czf backup-$DATE.tar.gz database/data website/public
docker-compose up -d
```

## Maintenance

### Regular Tasks

1. Update containers:
```bash
# Pull latest images
docker-compose pull

# Rebuild and restart services
docker-compose up -d --build
```

2. Monitor resources:
```bash
# Check container resource usage
docker stats

# View container logs
docker-compose logs -f [service-name]
```

### Troubleshooting

Common issues and solutions:

1. Container won't start:
   - Check logs: `docker-compose logs [service-name]`
   - Verify configuration files
   - Check resource availability

2. Performance issues:
   - Monitor resource usage
   - Check application logs
   - Adjust Colima resource allocation

## Best Practices

1. **Version Control**
   - Keep all configuration files in Git
   - Document changes and updates
   - Use environment variables for sensitive data

2. **Security**
   - Regular security updates
   - Use secrets management
   - Implement proper firewall rules
   - Regular security audits

3. **Monitoring**
   - Set up logging
   - Monitor resource usage
   - Track application metrics

## Next Steps

After basic setup:
1. Implement SSL/TLS certificates
2. Set up monitoring and alerting
3. Configure automatic backups
4. Implement CI/CD pipeline
5. Add additional services as needed

Remember to regularly:
- Update containers and base images
- Check logs for issues
- Verify backups
- Monitor resource usage
- Test security measures

This guide provides a foundation for your home server. Adjust configurations and resources based on your specific needs and hardware capabilities.
