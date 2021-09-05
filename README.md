## Laravel8 Practice

```sh
# Update env file
cp .env.example .env

# Start php and mysql containers
docker-compose up -d php
docker-compose exec php composer install

# Permission
sudo chown -R $USER vendor
sudo chgrp -R www-data storage bootstrap/cache
sudo chmod -R ug+rwx storage bootstrap/cache

# Migrations
docker-compose up -d mysql
docker-compose exec php php artisan migrate

# Run the rest of the containers
docker-compose up -d
```