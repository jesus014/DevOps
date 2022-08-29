# Creacion de jv en contenedor

Se crea el archivo Dockerfile, el cual se inicia y selecciona la imagen a ocupar, posteriormente se copia los archivos a las carpetas del contenedor:

*Se crea un usario y se agregan permisos.

*Se utiliza los parametros y variables de entorno.

*Se expone el puerto y se inicializa el cmd

```
FROM openjdk:8-jdk-alpine
#Create group and user 
RUN addgroup -S devopsc && adduser -S javams -G devopsc
USER javams:devopsc
ENV JAVA_OPTS=""
ARG JAR_FILE
ADD ${JAR_FILE} app.jar
 # use a volume is mor efficient and speed that filesystem
VOLUME /tmp
EXPOSE 8080
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar" ]

```
