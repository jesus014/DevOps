Desplegar la aplicacion de facturacion de la compañia, en los entornos de integracion,preproduccion, produccion, la instalacion debe de contar alta disponibilidad, excepto en la integracion.

solucion:

DevOps

Microservicios

```
usuario=> angular frontend=> microservice2 backend=> postgres
```

## Comandos usados en docker.

Documentacion:

https://docs.docker.com/compose/install/compose-plugin/#installing-compose-on-linux-systems

https://docs.docker.com/compose/compose-file/compose-versioning/

Instalacion de Docker Compose V2:

```
 curl -SL https://github.com/docker/compose/releases/download/v2.7.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```

```
curl -SL https://github.com/docker/compose/releases/download/v2.7.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```

Imagenes y contenedores:

**Descargar la imagen cualquiera**

```
docker pull ""nombre de imagen"
```

**Crear el contenedor billingapp**

```
docker run -p 80:80 -p  8080:8080 --name billingapp sotobotero/udemy-devops:0.0.1
```

**Listar los contenedores existentes**

```
docker ps -a
```

**Detener un contenedor**

```
docker stop nombreobjeto
```

**Ver los logs de un contenedor**

```
docker logs nombreobjeto
```

**Iniciar un contenedor detenido**

```
docker start nombreobjeto
```

**Listar las imagenes locales**

```
 docker image ls
```

**Eliminar una imagen**

```
docker image rm nombreobjeto
```

## Comandos usados en docker compose

Descargar las imagenes usndo docker-compose

```
docker-compose -f stackdb.yml pull
```

Inicar los contenedores

```
docker-compose -f stackdb.yml up -d
```

Url de la inteface de adminer

http://localhost:9090/

## crear una imagen:

Costruir la imagen

```
docker build -t billingapp:prod --no-cache --build-arg JAR_FILE=target/*.jar .
```

Levantar el contendor para probar

```
docker run -p 80:80 -p  8080:8080 --name billingapp billingapp:prod
```

Darle un nuevo tag

```
docker tag billingapp:prod sotobotero/udemy-devops:0.0.2
```

loguearse en el docker engine hacia docker hub

```
docker login (usas tu usario y contraseña)
```

hacer un push de la imagen

```
docker push sotobotero/udemy-devops:0.0.2
```

## Comandos Docker:


Eliminar todos los contenedores detenidos

```
docker system prune
```

Eliminar todas las imágenes

```
docker rmi $(docker images -a -q)
```

Listar los volumenes

```
docker volume ls
```

Eliminar todos los volumenes

```
docker volume prune
```

Construir las imagenes definidas en la orquestación

```
docker-compose -f stack-billing.yml build
```

Inicializar los contenedores de los servicios de la orquestación

```
docker-compose -f stack-billing.yml up -d
```

Detener todos los servicios de la orquestación

```
docker-compose -f stack-billing.yml sto
```

Detener todos los contenedores

```
docker stop $(docker ps -a -q)
```
