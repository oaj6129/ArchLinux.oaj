# Installing Docker and docker compose
### Commands to install Docker:
1. sudo apt update  
2. sudo apt install -y docker.io  
3. sudo systemctl start docker  
4. sudo systemctl enable docker  

### Commands to install Docker Compose:
1. sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 
  
2. sudo chmod +x /usr/local/bin/docker-compose

### Create a project directory
1. mkdir wordpress-docker  
2. cd wordpress-docker

### Create a docker yml file
1. touch docker-compose.yml
2. Paste the following contents into said file:  

***Paste this***  
  
version: '3.8'  

services:  
  wordpress:  
    image: wordpress:latest  
    container_name: wordpress  
    ports:  
      - "8080:80"  
    environment:  
      WORDPRESS_DB_HOST: db:3306  
      WORDPRESS_DB_USER: wordpress  
      WORDPRESS_DB_PASSWORD: wordpress  
      WORDPRESS_DB_NAME: wordpress  
    volumes:  
      - wordpress_data:/var/www/html  

  db:  
    image: mysql:5.7  
    container_name: mysql  
    environment:  
      MYSQL_DATABASE: wordpress  
      MYSQL_USER: wordpress  
      MYSQL_PASSWORD: wordpress  
      MYSQL_ROOT_PASSWORD: rootpassword  
    volumes:  
      - db_data:/var/lib/mysql  

volumes:  
  wordpress_data:  
  db_data:  

### Start the application
1. docker-compose up -d  
2. docker ps

### Access Word Press
Open your browser and navigate to http://localhost:8080. Follow the on-screen instructions to set up WordPress.
