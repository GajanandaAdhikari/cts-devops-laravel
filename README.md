# Laravel Docker Application

A complete Laravel application running with Nginx and MySQL using Docker Compose.

## 📋 Prerequisites

- Docker Desktop for Mac (compatible with M4 chip)
- Git
- Terminal/Command Line access

## 🏗️ Project Structure

```
cts/
├── docker/
│   ├── nginx/
│   │   └── default.conf       # Nginx configuration
│   └── php/
│       └── Dockerfile          # PHP-FPM Dockerfile
├── src/                        # Laravel application
├── docker-compose.yml          # Docker Compose configuration
├── .gitignore
└── README.md
```

## 🚀 Installation Steps

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd cts
```

### 2. Install Laravel Dependencies

```bash
# Install Laravel (first time only)
docker run --rm -v $(pwd)/src:/app composer create-project --prefer-dist laravel/laravel .

# Or if you already have the src folder with Laravel
docker run --rm -v $(pwd)/src:/app composer install
```

### 3. Configure Environment

```bash
# Copy environment file
cp src/.env.example src/.env

# Make sure database credentials match docker-compose.yml:
# DB_HOST=mysql
# DB_DATABASE=laravel
# DB_USERNAME=laravel_user
# DB_PASSWORD=laravel_password
```

### 4. Set Permissions

```bash
sudo chmod -R 775 src/storage
sudo chmod -R 775 src/bootstrap/cache
sudo chown -R $USER:$USER src/
```

### 5. Build and Start Containers

```bash
# Build Docker images
docker-compose build

# Start containers in detached mode
docker-compose up -d

# Check container status
docker-compose ps
```

### 6. Initialize Laravel

```bash
# Generate application key
docker-compose exec app php artisan key:generate

# Run database migrations
docker-compose exec app php artisan migrate

# Cache configuration
docker-compose exec app php artisan config:cache
```

### 7. Access the Application

Open your browser and visit: **http://localhost:8080**

## 🐳 Docker Services

| Service | Container Name | Port | Description |
|---------|---------------|------|-------------|
| app | laravel-app | 9000 | PHP-FPM 8.2 |
| nginx | laravel-nginx | 8080 | Nginx web server |
| mysql | laravel-mysql | 3306 | MySQL 8.0 database |

## 📝 Common Commands

### Docker Commands

```bash
# Start containers
docker-compose up -d

# Stop containers
docker-compose down

# View logs
docker-compose logs -f

# Rebuild containers
docker-compose up -d --build

# Access container shell
docker-compose exec app bash
```

### Laravel Artisan Commands

```bash
# Run migrations
docker-compose exec app php artisan migrate

# Create migration
docker-compose exec app php artisan make:migration create_users_table

# Clear cache
docker-compose exec app php artisan cache:clear
docker-compose exec app php artisan config:clear
docker-compose exec app php artisan route:clear
docker-compose exec app php artisan view:clear

# Run seeder
docker-compose exec app php artisan db:seed

# Create controller
docker-compose exec app php artisan make:controller UserController
```

### Composer Commands

```bash
# Install dependencies
docker-compose exec app composer install

# Update dependencies
docker-compose exec app composer update

# Require package
docker-compose exec app composer require package-name
```

## 🔧 Database Access

### Via Command Line

```bash
docker-compose exec mysql mysql -u laravel_user -p
# Password: laravel_password
```

### Connection Details

- **Host:** localhost
- **Port:** 3306
- **Database:** laravel
- **Username:** laravel_user
- **Password:** laravel_password

## 🛠️ Troubleshooting

### Containers won't start

```bash
# Check logs
docker-compose logs

# Remove and rebuild
docker-compose down -v
docker-compose up -d --build
```

### Permission issues

```bash
sudo chown -R $USER:$USER src/
sudo chmod -R 775 src/storage
sudo chmod -R 775 src/bootstrap/cache
```

### Database connection errors

1. Ensure MySQL container is running: `docker-compose ps`
2. Check database credentials in `src/.env`
3. Verify DB_HOST is set to `mysql` (not localhost)

### Port already in use

If port 8080 is already in use, edit `docker-compose.yml`:

```yaml
nginx:
  ports:
    - "8081:80"  # Change to any available port
```

## 📦 Project Features

- ✅ Laravel 10.x (latest)
- ✅ PHP 8.2 with FPM
- ✅ Nginx web server
- ✅ MySQL 8.0 database
- ✅ Docker Compose orchestration
- ✅ Persistent database storage
- ✅ Optimized for M4 MacBook Pro

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is open-sourced software licensed under the MIT license.

## 👤 Author

Your Name - [Your GitHub Profile]

## 🙏 Acknowledgments

- Laravel Framework
- Docker
- Nginx
- MySQL