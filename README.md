Dockerizando web apps - Docker El Salvador
==========================================

Ejemplo básico para desplegar una app con node + mongodb + nginx

Este proyecto contiene dos archivos de configuración y tres directorios, cada uno con un Dockerfile:

- `mongo/Dockerfile`: para crear una imagen de mongodb
- `nginx/Dockerfile`: para crear una imagen de nginx
- `node/Dockerfile`: para crear una imagen de node.js
- `init.sh`: descarga el proyecto de node de prueba para desplegar con docker
- `docker-compose.yml`: archivo para orquestar el despliegue de la app.

# Instrucciones

## Installar docker

Ver la documentación oficial para instalar docker en tu distro favorita: http://docs.docker.com/engine/installation/

### Instalar docker en debian-jessie:

Asegurarse de tener un kernel mayor o igual a 3.10 y tener un sistema operativo de 64 bits.

```
$ uname -r
```

Agregar gpg key

```
$ apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```

Agregar lo siguiente a `/etc/apt/sources.list.d/docker.list`:

```
# Debian Jessie
deb https://apt.dockerproject.org/repo debian-jessie main
```

Actualizar repositorios

```
$ sudo apt-get update
```

Verificar que apt obtiene docker del repositorio correcto:

```
$ sudo apt-cache policy docker-engine
```

Instalar docker

```
$ sudo apt-get install docker-engine
```

Iniciar el demonio Docker

```
$ sudo service docker start
```

Verificar que docker esta corriendo:

```
$ sudo docker run -t debian echo "Docker El Salvador"
```

## Descargar app de prueba

**Nota:** *es necesario tener instalado [git](https://git-scm.com/download/linux) para que el script no de problemas.*

El script `init.sh` no hace más que descargar un proyecto hecho con node.js, moverlo a la carpeta `node/` y editar el archivo `app.js` para que se conecte correctamente al contenedor de mongodb.

```
$ chmod +x init.sh && ./init.sh
```

## Desplegar Contenedores

Para desplegar los contendores necesitamos usar **docker-compose**, por lo tanto hay que instalarlo.

Descargar e instalar docker compose con **curl**

```
$ sudo curl -L https://github.com/docker/compose/releases/download/VERSION_NUM/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

Dar permisos de ejecución:

```
$ chmod +x /usr/local/bin/docker-compose
```

Probar la instalación

```
$ docker-compose --version
docker-compose version: 1.5.0
```

En la documentación oficial hay más información de como instalar docker-compose: https://docs.docker.com/compose/install/


### Desplegar app con docker-compose

**Nota:** *Usaremos el puerto 80 para este ejemplo, hay que asegurarse que ese puerto no este ocupado por otro servicio.*

La configuración necesaria para construir las imagenes de docker esta en el archivo ```docker-compose.yml```.

Construir las imagenes

```
$ sudo docker-compose build
```

Iniciar los Contenedores

```
$ sudo docker-compose up
```

Para probar que la app esta ejecutandose ir a: [localhost](http://localhost)

Para permitir que la app use un dominio como www.example.com, debemos editar el archivo ```/etc/hosts``` y agregar el dominio que queremos usar.

Para este ejemplo estamos usando un dominio de prueba llamado **www.node-app.dev** tal y como se ha configurado en: `nginx/nginx.conf`. Pasemos a editar `/etc/hosts` y agregamos lo siguiente:

```
127.0.0.5	node-app.dev www.node-app.dev
```

Probemos que esto ha funcionado, vamos a: [node-app.dev](http://node-app.dev)
