# Activacion de Docker swarm

Activacion de swarm

```
docker swarm  init
```

https://docs.docker.com/engine/swarm/

https://docs.docker.com/engine/reference/commandline/service_scale/

Crear el build con swarm

docker stack => comando para utilizar  swam

deploy =>

-c stack-billing.yml => el nombre del archivo que tomara.

billing => nombre que se le dara a la estructura creada.

```
docker stack deploy -c stack-billing.yml  billing
```

Revisar el estado de los servicios.

```
docker service ls
```

Revisar informacion deacuerdo al stack definido.

```
docker stack ps
```

eliminar todo el stack creado

```
docker stack rm billing
```

Salir del modo swarm.

```
docker swarm leave --force
```
