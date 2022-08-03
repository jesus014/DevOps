# Creacion de Angular en contenedor

Se crea el archivo Dockerfile, el cual se inicia y selecciona la imagen a ocupar, posteriormente se copia los archivos a las carpetas del contenedor:

*Se copia archivo de configuracion nginx.conf (contiene la configuracion basica de ng y configuracion del protocolo http server)

*Se expone el puerto y se inicializa el cmd

```
FROM nginx:alpine
 # use a volume is mor efficient and speed that filesystem
#Creacion de volumen. 
VOLUME /tmp
#comandos que se ejecutaran.
RUN rm -rf /usr/share/nginx/html/*
COPY nginx.conf /etc/nginx/nginx.conf
COPY billingApp /usr/share/nginx/html

#expose app and 80 for nginx app
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]


```
