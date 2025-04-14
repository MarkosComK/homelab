# HomeDrive: Personal Cloud Storage Server

A self-hosted cloud storage solution running on a Raspberry Pi 5, providing file sharing, user management, and web access to your personal data.

## Project Goals

- Create a fully-functional alternative to commercial cloud storage services
- Learn Docker containerization and microservices architecture
- Practice Linux server administration and networking
- Implement a secure and reliable backup solution
- Develop a web interface for easy file management

## System Architecture

This project uses Docker to create a modular, maintainable system with the following components:

1. **File Server**: For network file sharing and storage
2. **Database Server**: To manage users, permissions and file metadata
3. **Web Server**: To host the web interface
4. **Web Application**: The interface users will interact with
5. **VPN Server**: For secure remote access (future addition)

## Development Roadmap

### Phase 1: Basic Infrastructure
- [ ] Set up Docker and Docker Compose
- [ ] Implement Samba file server
- [ ] Set up Portainer for Docker management
- [ ] Create data volume structure
- [ ] Test basic file sharing

### Phase 2: Database Integration
- [ ] Add DB/SQL container
- [ ] Create database schema for users and files
- [ ] Implement backup routine for database
- [ ] Test database connectivity

### Phase 3: Web Interface
- [ ] Set up Nginx web server
- [ ] Develop basic web application (file listing, upload/download)
- [ ] Implement user authentication
- [ ] Connect web app to database and file system

### Phase 4: Advanced Features
- [ ] Add file sharing capabilities
- [ ] Implement file versioning
- [ ] Create mobile-friendly responsive design
- [ ] Add file preview for common file types

### Phase 5: Security & Remote Access
- [ ] Set up VPN server for remote access
- [ ] Implement SSL/TLS for web interface
- [ ] Add two-factor authentication
- [ ] Perform security audit

## Getting Started

### Prerequisites

- Docker and Docker Compose
- Git
- Basic Linux knowledge
- VM for initial development (transitioning to Raspberry Pi 5 later)

### Initial Setup

1. Clone the repository:
   ```bash
   git clone <your-repo-url>
   cd home-drive
   ```

2. Create necessary directories:
   ```bash
   mkdir -p volumes/shared volumes/backups volumes/database volumes/portainer
   ```

3. Start the base system:
   ```bash
   docker-compose up -d
   ```

4. Access Portainer for system management:
   ```
   http://your-server-ip:9000
   ```

## Container Setup

### File Server (SSH/SFTP)

- to do

### Database (to do)

### Web Server (to do)

## Database Schema
- to do

## Web Application

The web application will be built using:
- Backend: to do
- Frontend: to do
- Authentication: to do

### Key Features to Implement:
1. User registration and login
2. File browser with drag-and-drop upload
3. File sharing (public/private links)
4. Storage usage statistics
5. User profile management

## Migration to Raspberry Pi

Once development and testing are complete in the VM environment:

1. Install a Linux distribution on Raspberry Pi 5 (Ubuntu Server recommended)
2. Install Docker and Docker Compose:
3. Clone the repository to the Raspberry Pi
4. Copy data volumes from VM to Pi
5. Run docker-compose up -d on the Pi

## Troubleshooting

### Common Issues:

1. **Samba connection issues**:
   - Check firewall settings
   - Verify credentials

2. **Docker container not starting**:
   - Check logs: `docker-compose logs <service-name>`
   - Verify port conflicts: `netstat -tuln`
   - Check disk space: `df -h`

3. **Web application not accessible**:
   - Check Nginx configuration
   - Verify application logs
   - Test direct connection to application container

## Resources

- [Docker Documentation](https://docs.docker.com/)
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Raspberry Pi Documentation](https://www.raspberrypi.org/documentation/)

## License

