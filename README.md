# Docker WordPress Development Environment

A complete Docker-based WordPress development environment with MySQL and phpMyAdmin. Includes production-ready Docker images and optimization strategies.

## 🎯 What's Included

- **Development Environment**: Local WordPress with persistent data
- **Production Image**: Lightweight Docker image (891MB vs 7.9GB)
- **Docker Optimization**: Multiple approaches for different use cases
- **Duplicator Support**: Easy WordPress migration and deployment

## 🚀 Quick Start

1. **Clone this repository**
   ```bash
   git clone <your-repo-url>
   cd docker-wp
   ```

2. **Create environment file**
   ```bash
   cp .env.example .env
   # Edit .env with your preferred settings
   ```

3. **Start the containers**
   ```bash
   docker-compose up -d
   ```

4. **Access your sites**
   - WordPress: http://localhost:8000
   - phpMyAdmin: http://localhost:8080

## 📁 Project Structure

```
docker-wp/
├── docker-compose.yml          # Main development setup
├── docker-compose.production.yml # Production setup (lightweight)
├── docker-compose.image.yml    # Image-based setup
├── Dockerfile.dev              # Development image (7.9GB)
├── Dockerfile.production       # Production image (891MB)
├── .env                        # Environment variables
├── .gitignore                  # Git ignore rules
├── .dockerignore               # Docker ignore rules
└── wp-data/                    # WordPress files (not in git)
```

## 🔧 Configuration

### Environment Variables

Edit `.env` file to customize:

```env
# Database Configuration
MYSQL_ROOT_PASSWORD=rootpassword
MYSQL_DATABASE=wordpress
MYSQL_USER=wp_user
MYSQL_PASSWORD=wp_password

# WordPress Configuration
WORDPRESS_DB_HOST=database
WORDPRESS_DB_NAME=wordpress
WORDPRESS_DB_USER=wp_user
WORDPRESS_DB_PASSWORD=wp_password
WORDPRESS_TABLE_PREFIX=wp_
```

### Default Ports

- **WordPress**: 8000
- **phpMyAdmin**: 8080
- **MySQL**: 3306

## 🛠️ Docker Services

- **wordpress**: WordPress with Apache
- **database**: MySQL database
- **phpmyadmin**: Database management interface

## 📋 Available Commands

### Basic Commands
```bash
# Start containers
docker-compose up -d

# Stop containers
docker-compose down

# View logs
docker-compose logs

# Restart containers
docker-compose restart
```

### Development Commands
```bash
# Access WordPress container
docker exec -it docker-wp-wordpress-1 bash

# Access database container
docker exec -it docker-wp-database-1 mysql -u wp_user -p

# Check container status
docker-compose ps

# View real-time logs
docker-compose logs -f
```

### Image Management Commands
```bash
# Build development image (7.9GB)
docker build -f Dockerfile.dev -t docker-wordpress:local .

# Build production image (891MB)
docker build -f Dockerfile.production -t docker-wordpress:production .

# List all images
docker images

# Remove specific image
docker rmi docker-wordpress:local

# Clean up unused images
docker image prune -a
```

### Testing Commands
```bash
# Test development image
docker run -d --name test-dev -p 8090:80 docker-wordpress:local

# Test production image
docker run -d --name test-prod -p 8091:80 docker-wordpress:production

# Stop test containers
docker stop test-dev test-prod
docker rm test-dev test-prod
```

## 🔄 WordPress Migration (Duplicator)

This setup supports WordPress migration using Duplicator plugin:

1. Place your Duplicator archive and installer files in `wp-data/`
2. Access installer at: http://localhost:8000/installer.php
3. Use database connection details from your `.env` file

## 🐳 Docker Image Optimization

### Image Size Comparison
- **Development Image**: 7.9GB (includes entire WordPress installation)
- **Production Image**: 891MB (88% smaller, mounts content separately)

### Building Docker Images

#### Development Image (Full Site)
```bash
docker build -f Dockerfile.dev -t docker-wordpress:local .
# Results in 7.9GB image with complete WordPress installation
```

#### Production Image (Lightweight)
```bash
docker build -f Dockerfile.production -t docker-wordpress:production .
# Results in 891MB image, mounts wp-content as volume
```

### Testing Your Images
```bash
# Test development image
docker run -d --name test-dev -p 8090:80 docker-wordpress:local

# Test production image
docker run -d --name test-prod -p 8091:80 docker-wordpress:production
```

### Different Deployment Strategies

#### 1. Development (Current Setup)
```bash
docker compose up -d
# Uses wp-data folder for persistent development
```

#### 2. Production (Lightweight)
```bash
docker compose -f docker-compose.production.yml up -d
# Uses lightweight image + mounted wp-content
```

#### 3. Image-Based (Portable)
```bash
docker compose -f docker-compose.image.yml up -d
# Uses built image with everything included
```

### Cleaning Up Images
```bash
# Remove specific images
docker rmi docker-wordpress:local
docker rmi docker-wordpress:production

# Remove all unused images
docker image prune -a
```

## 🚨 Troubleshooting

### Port Conflicts
If ports are already in use, modify them in `docker-compose.yml`:
```yaml
ports:
  - "8001:80"  # Change 8000 to 8001
```

### Database Connection Issues
Ensure database container is running:
```bash
docker-compose ps
docker-compose logs database
```

### Permission Issues
If WordPress can't write files:
```bash
docker exec docker-wp-wordpress-1 chown -R www-data:www-data /var/www/html
```

### Large Image Sizes
If your Docker images are too large:
1. Use the production Dockerfile (891MB vs 7.9GB)
2. Mount wp-content as volume instead of including in image
3. Clean up unused images: `docker image prune -a`

### Container Name Conflicts
If you get "container name already in use":
```bash
docker ps -a                    # List all containers
docker rm container_name       # Remove specific container
docker container prune         # Remove all stopped containers
```

## 📚 Additional Resources

- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [WordPress Docker Image](https://hub.docker.com/_/wordpress/)
- [MySQL Docker Image](https://hub.docker.com/_/mysql/)
- [phpMyAdmin Docker Image](https://hub.docker.com/_/phpmyadmin/)

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).