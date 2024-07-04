# Docker Commands

## Comandos básicos
### Imagen
docker build -t name-image:tag . (construye la imagen con el directorio actual)
docker images (lista las imágenes)
docker pull name-image (descarga la imagen)
docker run name-image -d (busca la imagen, si no existe la descarga, luego crea un contenedor, por último inicia el contenedor)
docker image rm name-image (elimina la imagen)
docker image rm -f name-image (elimina la imagen forzando)

### Contenedor
docker ps (lista los contenedores)
docker ps -a (lista todos los contenedores)
docker pwd (muestra la ruta del contenedor)
docker create -it --name name-container name-image (crea un contenedor interactivo)
docker create -p 8080:8080 --name name-container name-image (primer número hace referencia al puerto del host, segundo número hace referencia al puerto del contenedor, crea un contenedor con un puerto)
docker create -p8080 --name name-container name-image (el número hace referencia al puerto del contenedor y le asigna un puerto aleatorio al host, crea un contenedor con un puerto)
docker create -p8080:8080 --name name-container -e "ENV_VAR=value" -e "ENV_VAR2=value2" name-image (crea un contenedor con variables de entorno)
docker create -p8080:8080 --name name-container --network name-network -e "ENV_VAR=value" -e "ENV_VAR2=value2" name-image (crea un contenedor con variables de entorno y redes)
docker run --name name-container -p8080(host):8080(contenedor) -d name-image (crea un contenedor cada vez que se ejecuta el comando y lo inicia)
docker start name-container (inicia el contenedor)
docker stop name-container (para el contenedor)
docker restart name-instance (reiniciar el contenedor)
docker rm name-container (elimina el contenedor)

### Docker compose (resume los comandos para crear una red de contenedores)
docker compose up (construye todos los contenedores del archivo docker-compose.yml, los inicia y construye una imagen en base a los contenedores)
docker compose down (elimina los contenedores, la imagen y la red, todo lo que se encuentra relacionado con el archivo docker-compose.yml)

<details>
  <summary>docker-compose.yml (archivo de configuración)</summary>

```yml
version: "3.9" # actualmente se usa la versión **3** por defecto
services:
  name-contenedor: #nombre del contenedor
    build: . #construye la imagen con el directorio actual
    ports: #puerto host:puerto contenedor
      - "3000:3000"
    links: #enlace, relaciona el contenedor con otro contenedor, crea una red de contenedores
      - name-contenedor2
  name-contenedor2: #nombre del contenedor
    image: name-image #imagen a usar
    ports: #puerto host:puerto contenedor
      - "3001:3001"
    environment: #variables de entorno
      - ENV_VAR=value
      - ENV_VAR2=value2
    volumes: #volumenes para el almacenamiento de datos en el host y en el contenedor
      - name-volume:/home/app/volume  #nombre del volumen en el host y en el contenedor
      # mongodb -> /data/db #mapeo de directorio en el contenedor
      # mysql -> /var/lib/mysql #bis 
      # postgres /var/lib/postgresql/data #bis  
      # mariadb -> /var/lib/mysql #bis
volumes: #volumenes para el almacenamiento de datos en el host y en el contenedor **Esto ya no es necesario en la versión 3**
  name-volume: #nombre del volumen en el host y en el contenedor
```
</details>
 
### Volumen
docker volume ls (lista los volumenes)
docker volume rm name-volume (elimina el volumen)

### Volumen en instancia
docker volume ls -f dangling=true (lista los volumenes en instancia)
docker volume rm name-volume (elimina el volumen)

## Comandos avanzados
docker volume inspect name-volume (muestra información del volumen)
docker volume inspect -f '{{.Mountpoint}}' name-volume (muestra la ruta del volumen)
docker logs name-container (muestra los logs del contenedor)
docker logs --follow name-container (muestra los logs del contenedor y sigue actualizando)
docker network ls (lista las redes)
docker network create name-network (crea una red)
docker network rm name-network (elimina una red)

## Dockerfile
```dockerfile
FROM node:18 (imagen base, se puede usar cualquier imagen que se encuentre en docker hub)

RUN mkdir -p /home/app (ruta dentro del mismo contenedor para almacenar nuestro código fuente)

COPY . /home/app (copia el directorio actual del host al directorio dentro del contenedor)

EXPOSE 3000 (puerto que se abrirá en el host para el contenedor)

CMD ["node", "/home/app/index.js"] (comando que se ejecutará cuando se inicie el contenedor, la ruta es relativa al directorio dentro del contenedor)
```

## Dockerfile.dev (archivo de configuración con nombre perzonalizado)
- docker compose -f docker-compose-dev.yml up (-f indica el archivo custom que se va a usar en lugar del archivo docker-compose.yml)

```dockerfile
FROM node:18

RUN npm i -g nodemon (instala globalmente los paquetes de npm y nodemon para reiniciar el contenedor en caso de que se produzca un cambio en el código fuente)
RUN mkdir -p /home/app  (crea el directorio home/app dentro del contenedor)

WORKDIR /home/app (cambia el directorio de trabajo del contenedor)

EXPOSE 3000 (puerto que se abrirá en el host para el contenedor)

CMD ["nodemon", "index.js"] (comando que se ejecutará cuando se inicie el contenedor, la ruta es relativa al directorio dentro del contenedor)
```

<details>
  <summary>docker-compose-dev.yml (archivo de configuración con nombre perzonalizado)</summary>

```yml
version: "3.9"
services:
  name-contenedor:
    build:
      context: . #ruta actual
      dockerfile: dockerfile.dev #ruta al Dockerfile
    ports:
      - "3000:3000"
    links:
      - name-contenedor2
    volumes:
      - .:/home/app #"." volumen anonimo para el directorio actual, ":" indica la ruta de destino dentro del contenedor) 

  name-contenedor2:
    image: name-image
    ports:
      - "3001:3001"
    environment:
      - ENV_VAR=value
      - ENV_VAR2=value2
    volumes: 
      - name-volume:/home/app/volume
      <!-- mongodb -> /data/db -->
      <!-- mysql -> /var/lib/mysql -->
      <!-- postgres -> /var/lib/postgresql/data -->
volumes:
  name-volume:
```
</details>

<details>
  <summary>Docker compose con las imagenes de php-apache y mariadb</summary>

## compose.yml
```yml
version: '3'
services:
  phpapp:
    build:
      context: .
      dockerfile: dockerfile
    ports:
      - "80:80"
    volumes:
      - ./htdocs:/var/www/html
  mariadb: 
    build:
      context: .
      dockerfile: dockerfile2
    ports:
      - "3306:3306"
    volumes:
      - ./db:/var/lib/mysql
    environment:
      - MARIADB_ALLOW_EMPTY_ROOT_PASSWORD=1
    command: ['mariadbd', '--sql_mode=NO_ENGINE_SUBSTITUTION', '--innodb-strict-mode=0']
```

## dockerfile
```dockerfile
FROM php:8.2.4-apachet

RUN docker-php-ext-install pdo pdo_mysql
RUN docker-php-ext-install mysqli
RUN docker-php-ext-enable mysqli
RUN a2enmod rewrite
RUN a2enmod headers
```	

## dockerfile2
```dockerfile
FROM mariadb:11.2.4-jammy

COPY ./backup_sql_03-07-2024 /docker-entrypoint-initdb.d (migración de datos desde una carpeta con los archivos sql a el directorio en el contenedor de mariadb una vez que se inicie el contenedor)
```
</details>