## Links
**Imagem docker**
  https://quay.io/repository/keycloak/keycloak?tab=info

**Notes**
  Ao linkar com um banco de dados o keyCloak já cria todas as tabelas necessárias para o star inicial da aplicação

## Configurar docker compose
```yaml
version: "3"

services:
  keycloak:
    image: quay.io/keycloak/keycloak:21.1
    command:
      - start-dev
    ports:
      - 8080:8080
    environment:
      - KEYCLOAK_ADMIN=admin
      - KEYCLOAK_ADMIN_PASSWORD=admin
      - KC_DB=mysql
      - KC_DB_URL=jdbc:mysql://db:3306/keycloak
      - KC_DB_USERNAME=root
      - KC_DB_PASSWORD=root
    depends_on:
      db:
        condition: service_healthy

  db:
    platform: linux/x86_64
    image: mysql:8.0.30-debian
    volumes:
      - ./.docker/dbdata:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=root
      - MYSQL_DATABASE=keycloak
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 20s
      retries: 3

```

## Run
**Dev/Local**
```console
#run container
docker compose up

#drop container
docker compose down
```

## Docker CLI
```console
#run container bash mode for container name -> db
docker compose exec db bash


```

## Mysql CLI
```console
#connect user
mysql -uroot -root

#show databases
show databases;

#use database <name>
use keycloak;


```