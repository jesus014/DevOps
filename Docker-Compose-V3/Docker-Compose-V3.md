Creacion del archivo yml, el cual contiene los demas servicios que seran ocupados.

## Servicio de la base de base de datos.

*Se crea el nombre del servicio

*se utiliza la imagen de postgres, la ultima

*puertos

*volumenes que seran utilizados #allow *.sql, *.sql.gz, or *.sh and is execute only if data directory is empty

*variables de entorno que son utilizadas.

*Se declaran los dos ambientes de produccion y proproduccion.

*Se utiliza la red.


```
version: '3.1'

services:
#database engine service
  postgres_db_prod:
    container_name: postgres_prod
    image: postgres:latest
    restart: always
    networks:
      - env_prod 
    ports:
      - 5432:5432
    volumes:
        #allow *.sql, *.sql.gz, or *.sh and is execute only if data directory is empty
      - ./dbfiles:/docker-entrypoint-initdb.d
      - /var/lib/postgres_data_prod:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: qwerty
      POSTGRES_DB: postgres  
#database engine service
  postgres_db_prep:
    container_name: postgres_prep
    image: postgres:latest
    restart: always
    networks:   
      - env_prep
    ports:
      - 4432:5432
    volumes:
        #allow *.sql, *.sql.gz, or *.sh and is execute only if data directory is empty
      - ./dbfiles:/docker-entrypoint-initdb.d
      - /var/lib/postgres_data_prep:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: qwerty
      POSTGRES_DB: postgres  
```


## Servicio para adminer para poder ver la bd.

*Se crea el nombre del servicio

*se utiliza la imagen de adminer

*Depende que el servicio se alla terminado de inicializar.

*puertos.

*Utilizando la red que se crearon.


```
#database admin service
#Use for All enviroments
  adminer:
    container_name: adminer
    image: adminer
    restart: always
    networks:
      - env_prod
      - env_prep
    depends_on: 
      - postgres_db_prod
      - postgres_db_prep   
    ports:
       - 9090:8080



```


## Servicio para produccion del proyecto.

*compilar build: se tomar el dockerfile que se encuentra en la carpeta java.

*args => argumentos que se envian.

*se envian las variables de entorno.

*depende que el otro servicio este completamente iniciado.

*se delimitan los puertes

*Se limita el uso de memoria del contenedor.


```
#ENV_PROD
#Billin app backend service
  billingapp-back-prod:
    build:
      context: ./java
      args:
        - JAR_FILE=billing-0.0.3-SNAPSHOT.jar
    networks:
      - env_prod   
    container_name: billingApp-back-prod   
    environment:
       - JAVA_OPTS=
         -Xms256M 
         -Xmx256M     
    depends_on:   
      - postgres_db_prod
    ports:
      - 8080:8080 
#Billin app frontend service
  billingapp-front_prod:
    build:
      context: ./angular 
    networks:
      - env_prod  
    deploy:  
        resources:
           limits: 
              cpus: "0.15"
              memory: 250M
#recusos dedicados, mantiene los recursos disponibles del host para el contenedor
           reservations:
              cpus: "0.1"
              memory: 128M
    #container_name: billingApp-front
    depends_on:   
      - billingapp-back-prod
#rango de puertos para escalar  
    ports:
      - 8081-8082:80

```


## Servicio para pre-produccion del proyecto.

*compilar build: se tomar el dockerfile que se encuentra en la carpeta java.

*args => argumentos que se envian.

*se envian las variables de entorno.

*depende que el otro servicio este completamente iniciado.

*se delimitan los puertos.

*Se limita el uso de memoria del contenedor.



## Configuracion de red en el proyecto.

*Se declara las redes en modo produccion

*Se declara las redes en modo pre-produccion.


```
networks:
  env_prod:
    driver: bridge  
    #activate ipv6
    driver_opts: 
            com.docker.network.enable_ipv6: "true"
    #IP Adress Manager
    ipam: 
        driver: default
        config:
        - 
          subnet: 172.16.232.0/24
          gateway: 172.16.232.1
        - 
          subnet: "2001:3974:3979::/64"
          gateway: "2001:3974:3979::1"
  env_prep:   
    driver: bridge  
    #activate ipv6
    driver_opts: 
            com.docker.network.enable_ipv6: "true"
    #IP Adress Manager
    ipam:
        driver: default
        config:
        - 
          subnet: 172.16.235.0/24
          gateway: 172.16.235.1
        - 
          subnet: "2001:3984:3989::/64"
          gateway: "2001:3984:3989::1"
```
