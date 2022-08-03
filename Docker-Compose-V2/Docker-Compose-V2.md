Creacion del archivo yml, el cual contiene los demas servicios que seran ocupados. 

## Servicio de la base de base de datos.

*Se crea el nombre del servicio

*se utiliza la imagen de postgres, la ultima 

*puertos 

*volumenes que seran utilizados #allow *.sql, *.sql.gz, or *.sh and is execute only if data directory is empty

*variables de entorno que son utilizadas.

```
database engine service
  postgres_db:
    container_name: postgres
    image: postgres:latest
    restart: always
    ports:
      - 5432:5432
    volumes:
        #allow *.sql, *.sql.gz, or *.sh and is execute only if data directory is empty
      - ./dbfiles:/docker-entrypoint-initdb.d
      - /var/lib/postgres_data:/var/lib/postgresql/data
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

```
#database admin service
  adminer:
    container_name: adminer
    image: adminer
    restart: always
    depends_on: 
      - postgres_db
    ports:
       - 9090:8080
```

## Servicio para backend del proyecto.

*compilar build: se tomar el dockerfile que se encuentra en la carpeta java.

*args => argumentos que se envian.

*se envian las variables de entorno.

*depende que el otro servicio este completamente iniciado.

*se inicia el puerto bajo 8080

```
#Billin app backend service
  billingapp-back:
    build:
      context: ./java
      args:
        - JAR_FILE=*.jar
    container_name: billingApp-back  
    environment:
       - JAVA_OPTS=
         -Xms256M 
         -Xmx256M     
    depends_on:   
      - postgres_db
    ports:
      - 8080:8080 
```

## Servicio para fron del proyecto.

*compilar build: se tomar el dockerfile que se encuentra en la carpeta angular.

*se asigna el nombre del contenedor.

*depende que el otro servicio este completamente iniciado.

*se inicia el puerto bajo 80

```
  billingapp-front:
    build:
      context: ./angular 
    container_name: billingApp-front
    depends_on:   
      - billingapp-back
    ports:
      - 80:80 
```
