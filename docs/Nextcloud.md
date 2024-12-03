Instal·lació de NextCloud Docker

## Crear arxiu Docker Compose

#### Crea un directori per a NexCloud
````bash
mkdir -p ~/nextcloud-docker cd ~/nextcloud-docker
````

#### Crear docker -compose.yml
````bash
nano docker-compose.yml
````

#### Afegir Contingut
Canviar contrasenya Base de dades: root_password
Canviar  usuari i contrasenya NextCloud: nextcloud_user i nexcloud_password
````yaml
version: '3.8'

services:
  db:
    image: mariadb:latest
    container_name: nextcloud-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: nextcloud
      MYSQL_USER: nextcloud_user
      MYSQL_PASSWORD: user_password
    volumes:
      - db_data:/var/lib/mysql

  app:
    image: nextcloud:latest
    container_name: nextcloud-app
    restart: always
    ports:
      - 8080:80
    environment:
      MYSQL_HOST: db
      MYSQL_DATABASE: nextcloud
      MYSQL_USER: nextcloud_user
      MYSQL_PASSWORD: user_password
    volumes:
      - nextcloud_data:/var/www/html
    depends_on:
      - db

volumes:
  db_data:
  nextcloud_data:
````

## Iniciar el contenidor
````bash
sudo docker-compose up -d
````

## Configurar NextCloud en el navegador
Obre un navegador i ves a http://<IP_DEL_SERVIDOR>:8080.                 
Completa la configuració:                             
	Usuari administrador de Nextcloud.                   
	Base de dades:                  
		Servidor de base de dades: db.                  
		Nom de la base de dades: nextcloud.                 
		Usuari: nextcloud_user.            
		Contrasenya: user_password.