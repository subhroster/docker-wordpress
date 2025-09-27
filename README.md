# Docker WordPress Development Environment

A complete Docker-based WordPress development environment with MySQL and phpMyAdmin.

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
├── docker-compose.yml    # Docker services configuration
├── .env                  # Environment variables (not in git)
├── .gitignore           # Git ignore rules
├── .dockerignore        # Docker ignore rules
└── wp-data/             # WordPress files (not in git)
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

```bash
# Start containers
docker-compose up -d

# Stop containers
docker-compose down

# View logs
docker-compose logs

# Restart containers
docker-compose restart

# Access WordPress container
docker exec -it docker-wp-wordpress-1 bash

# Access database container
docker exec -it docker-wp-database-1 mysql -u wp_user -p
```

## 🔄 WordPress Migration (Duplicator)

This setup supports WordPress migration using Duplicator plugin:

1. Place your Duplicator archive and installer files in `wp-data/`
2. Access installer at: http://localhost:8000/installer.php
3. Use database connection details from your `.env` file

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