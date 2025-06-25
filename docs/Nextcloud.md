# Instal·lació de Nextcloud amb Docker

- Consultar la [Installar Docker en Ubuntu](Docker_Ubuntu_24-04.md)
## 1. Crear el directori de treball

```bash
mkdir -p ~/nextcloud-docker
cd ~/nextcloud-docker
```

## 2. Crear el fitxer `docker-compose.yml`

```bash
nano docker-compose.yml
```

## 3. Afegir el contingut següent

> **Important:** Substitueix `root_password`, `nextcloud_user`, i `user_password` per valors segurs i personals.

```yaml
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
```

## 4. Iniciar els contenidors

```bash
sudo docker-compose up -d
```

## 5. Configurar Nextcloud des del navegador

1. Obre el navegador i accedeix a: [http://<IP_DEL_SERVIDOR>:8080](http://<IP_DEL_SERVIDOR>:8080)
2. Completa la configuració:
    - **Usuari administrador de Nextcloud:** (tria un nom d'usuari i contrasenya)
    - **Base de dades:**
        - **Servidor de base de dades:** `db`
        - **Nom de la base de dades:** `nextcloud`
        - **Usuari:** `nextcloud_user`
        - **Contrasenya:** `user_password`

---

### Consells addicionals

- Per aturar els contenidors:
  ```bash
  sudo docker-compose down
  ```
- Per veure els logs:
  ```bash
  sudo docker-compose logs -f
  ```
- Consulta la [documentació oficial de Nextcloud Docker](https://hub.docker.com/_/nextcloud) per a més opcions de configuració.