## Laravel 8 Practice

> Under development

Dockerized Laravel 8 playgrounds

```sh
# Update env file
cp .env.example .env

### DANGER: DONT FORGET TO MAKE ADJUSTMENTS TO YOUR ENV FILE ###

# Build and create php containers
# 'web' contains supervisor which runs php-fpm, laravel-websockets, queue-worker and cron
docker-compose build --no-cache web
docker-compose up -d web
docker-compose exec web composer install

# Permission
sudo chown -R $USER vendor
sudo chgrp -R www-data storage bootstrap/cache
sudo chmod -R ug+rwx storage bootstrap/cache

# Create your app key
docker-compose exec web php artisan key:generate

# Migrations
docker-compose up -d mysql
docker-compose exec web php artisan migrate

# Run the rest of the containers
docker-compose up -d

#### EXTRAS ####

# Compile node packages, minify
docker-compose exec -u $USER web yarn
docker-compose exec -u $USER web yarn prod

# If you want to generate files
docker-compose exec -u $USER web php artisan make:controller MyVeryFirstController
```

## TODO
* How to setup custom domain + HTTPS via `mkcert`
* Emulate AWS S3 API with Minio