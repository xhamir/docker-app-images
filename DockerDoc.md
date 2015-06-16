Docker 
===================

**Herramientas:**
-------------
Se usan las usaron las siguientes herramientas:

> - **Docker**
> - **Docker-machine**
> - **Docker-compose(fig)**
> - **Docker-swarm**

----------


<i class="icon-wrench"></i> **Instalación**
--------

### **Mac Os X**
Se pueden utilizar las siguientes herramientas para trabajar con docker sobre Mac Os X:
> - **Docker-machine**(yo opté por esta opción)
> - **Kitematic**
> - **Boot2Docker**

Herramientas docker:
> - **Docker-machine**
> - **Docker-compose(fig)**
> - **Docker-swarm**



***Docker-Machine:***
>Dependencias:

> - VirtualBox o VMWare

>Utilizando VirtualBox nos encontramos con un problema el cual es que tiene problemas al crear el puente que comunicara docker:
>>INFO[0000] Waiting for VM to start...

>1. Solución: Instalar la versión más nueva pero aun esta en desarrollo.
>2. Solución: Crear 2 o más VM al mismo tiempo esperando que en alguna si funcione(eliminar inmediatamente aquellas VM que no funcionaran correctamente).

```
Release Install -Version 0.2.0
//Run in terminal
curl -L https://github.com/docker/machine/releases/download/v0.2.0/docker-machine_darwin-amd64 > /usr/local/bin/docker-machine
chmod +x /usr/local/bin/docker-machine
```

```
Pre-Release Install -Version 0.3.0-rc2
//Run in terminal
curl -L https://github.com/docker/machine/releases/download/v0.3.0-rc2/docker-machine_darwin-amd64 > /usr/local/bin/docker-machine
chmod +x /usr/local/bin/docker-machine
```
***Docker-Compose:***
```
Install -Version 1.2.0
//Run in terminal
curl -L https://github.com/docker/compose/releases/download/1.2.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```
***Docker-Swarm:*** En este caso se utiliza una imagen que se encuentra en dockerhub corriéndolo como un contenedor. 
```
docker pull swarm
```

### **Ubuntu**

En este caso se pueden usar tanto **docker-machine** como instalar directamente **docker([instalar](http://docs.docker.com/installation/ubuntulinux/))**(no recomendable instalar ambos):

***Docker-Machine:***
>Dependencias:

> - VirtualBox o VMWare

```
Release Install -Version 0.2.0
//Run in terminal
curl -L https://github.com/docker/machine/releases/download/v0.2.0/docker-machine_linux-amd64 > /usr/local/bin/docker-machine
chmod +x /usr/local/bin/docker-machine
```

***Docker-Swarm:*** En este caso se utiliza una imagen que se encuentra en dockerhub corriéndolo como un contenedor. 
```
docker pull swarm
```

### **Windows**

En este caso se usa **Windows Docker Client([instalar](http://docs.docker.com/installation/windows/))**:



<i class="icon-beaker"></i> **Comandos**
--------
### **Docker-Machine**
Creación de VM's:

>VirtualBox:
>```
>docker-machine create -d virtualbox dev
>```

>VMWare:
>```
>docker-machine create --driver vmwarefusion --vmwarefusion-boot2docker-url "https://github.com/cloudnativeapps/boot2docker/releases/download/v1.6.0-vmw/boot2docker-1.6.0-vmw.iso" --vmwarefusion-memory-size 4096 dev
>```


Comandos Generales:
```
docker-mahine ls //Te muestra las máquinas que haz creado.
docker-machine env dev // Te muestra el siguiente comando.
eval "$(docker-machine env dev)" // Este comando hace el puente entre tu local y la VM creada para utilizar docker.
docker-machine ip // Te muestra la ip donde se estarán exponiendo todo puerto de las apps dentro de tu docker
docker-machine stop <nombre>//Detiene la VM
docker-machine start <nombre>// Iniciar la VM
```


### **Docker**

Comandos Generales de docker:
```
docker pull <imagen>//Descargar de dockerhub una imagen
docker images // Ver imágenes locales 
docker ps //Funciona para ver los contenedores activos
docker rmi <id>// Elimina imágenes
docker rm <id o nombre>//Elimina contenedores 
docker push <imagen> //Subir la imagen que creaste
docker kill <id o nombre>//Mata contenedores activos
docker info //Ver cuantos contenedores e imágenes hay así como información de tu host docker
docker start <id o nombre>//Arrancar contenedor en pausa
docker stop <id o nombre>//Detener contenedor
```
Comandos importantes:

> **docker run** 
> Este comando convierte las imágenes en contenedores en funcionamiento y tiene diferentes opciones como puertos, nombre, tag, variables de entorno, volúmenes, directorio de trabajo, entre otras configuraciones del contenedor.  

Ejemplos:
```
docker run -d -P nginx 
//-d activa el contenedor sin interacción con el usuario de background
//-P expondra puertos aleatorios
docker run -it nginx bash 
//-it activa el contenedor con interacción con el usuario
//bash en este caso la interacción sera con el bash del contenedor dependiendo de la base de la imagen sera el bash del os utilizado.
docker run -d -p 5000:5000 nginx
//-p 5000:5000 se expuso en el puerto 5000 
```

> **docker attach** 
> Este comando corre un comando dentro de un contenedor activo. 

Ejemplo:
```
docker attach nginx
```

> **docker build** 
> Este comando crea una imagen a partir de un Dockerfile. 

Ejemplo:
```
docker build .
//Con el . tomara la imagen que este en la carpeta(se puede poner la dirección del Dockerfile)
```

Comandos Útiles.

```
Limpieza docker
docker rm -f $(docker kill $(docker ps -q -a)) //Eliminar todos los contenedores
docker rmi -f $(docker images -q -a) //Elimina todas las imágenes 
//Puedes agregar -v para eliminar los volúmenes que creaste con información persistente.
```

### **Dockerfile**

Dockerfile es un archivo que contiene las acciones que se realizaran para crear una imagen.

Ejemplos:

```
#Ubuntu based hello world
FROM ubuntu:15.04 # FROM es en lo que esta basada la imagen que se creara no necesariamente tiene que ser un OS puede ser de otra imagen con un OS con sus propias configuraciones
MAINTAINER xhamir.one@gmail.com # El autor
RUN apt-get update # RUN son acciones que se ejecutaran al crear la imagen 
RUN apt-get install -y nginx # Cada línea se convierte en un layer a la cual puedes accesar y modificar por ejemplo entrar a este layer tendrá todas las acciones generadas hasta este punto.
RUN apt-get install -y golang 
CMD ["echo", "Hello world"] # CMD es el comando que se ejecutara cuando la imagen este activa.
```


```
FROM xhamir/debian-base-app:0.01 //FROM es en que se basa la imagen que se creara
MAINTAINER Omar Jair Montalvo Aquines <xhamir.one@gmail.com> // Autor

#ENV NODE_VERSION='0.12.2' \ 
# NPM_VERSION='2.9.0' \
# RUBY_MAJOR='2.1' \
# RUBY_VERSION='2.1.6' \
# RUBY_DOWNLOAD_SHA256='1e1362ae7427c91fa53dc9c05aee4ee200e2d7d8970a891c5bd76bee28d28be4' 
#ENV son para la declaración de variables


# Instalación de dependencias de ruby
RUN gpg --keyserver pool.sks-keyservers.net \
    --recv-keys 7937DFD2AB06298B2293C3187D33FF9D0246406D \
    114F43EE0176B71C7BC219DD50A3051F888C628D

RUN curl -SLO "http://nodejs.org/dist/v0.12.2/node-v0.12.2-linux-x64.tar.gz" \
  && curl -SLO "http://nodejs.org/dist/v0.12.2/SHASUMS256.txt.asc" \
  && gpg --verify SHASUMS256.txt.asc \
  && grep " node-v0.12.2-linux-x64.tar.gz\$" SHASUMS256.txt.asc | sha256sum -c - \
  && tar -xzf "node-v0.12.2-linux-x64.tar.gz" -C /usr/local --strip-components=1 \
  && rm "node-v0.12.2-linux-x64.tar.gz" SHASUMS256.txt.asc \
  && npm install -g npm@"2.9.0" \
  && npm cache clear \ 
  && apt-get update \
  && apt-get install -y bison libgdbm-dev ruby systemtap-sdt-dev \
  && rm -rf /var/lib/apt/lists/* \
  && mkdir -p /usr/src/ruby \
  && curl -fSL -o ruby.tar.gz "http://cache.ruby-lang.org/pub/ruby/2.1/ruby-2.1.6.tar.gz" \
  && echo "1e1362ae7427c91fa53dc9c05aee4ee200e2d7d8970a891c5bd76bee28d28be4 *ruby.tar.gz" | sha256sum -c - \
  && tar -xzf ruby.tar.gz -C /usr/src/ruby --strip-components=1 \
  && rm ruby.tar.gz \
  && cd /usr/src/ruby \
  && autoconf \
  && ./configure --disable-install-doc --enable-dtrace \
  && make -j"$(nproc)" \
  && make install \
  && apt-get purge -y --auto-remove bison libgdbm-dev ruby systemtap-sdt-dev \
  && rm -r /usr/src/ruby 

# Instalación de bundler
RUN gem install bundler --no-rdoc --no-ri

# '/app/vendor/bundle' carpeta donde se guardaran las gemas - archivos persistentes
# Path original puedes verificarlo con 'gem env'
ENV GEM_HOME /app/vendor/bundle

# Set the image user as the unprivileged 'app' user:
USER app

# Set the image's working directory to the '/app' folder, which is the 'app'
# user's home:
WORKDIR /app

# Set BUNDLE_APP_CONFIG to GEM_PATH and set PATH to include app binaries:
ENV BUNDLE_APP_CONFIG=$GEM_HOME PATH=/app/bin:$GEM_HOME/bin:$PATH

CMD ["irb"]
```

```
# Define '/app' como volume que funciona para que los archivos de la app sean persistentes.
VOLUME /app
```

A considerar al crear un Dockerfile:
> Utilizar la menor cantidad de líneas ya que cada línea es un **layer** que es parte de un historial de acciones dentro de la imagen haciendo más pesada la imagen.
> 
> Sin perder la claridad de la imagen para facilitar su mantenimiento.

### **Docker-Compose**

Es la manera de definir y correr aplicaciones complejas con Docker. Con compose defines varios contenedores en un solo archivo.

Se crea un archivo ```docker-compose.yml``` en el cual definirás tus servicios.

Ejemplo:
```
# Servicio de Redis:
redis:
  image: redis:2.8.12 #Al momento de crear un servicio se puede seleccionar una imagen de dockerhub
  #O se puede crear una propia usando build: .
  ports:
    - "6390:6379" # Bind host port 6390 to redis port 6379

# Servicio de MongoDB:
mongodb:
  image: mongo:3
  command: --smallfiles
  ports:
    - "27028:27017" # Bind host port 27028 to MongoDB port 27017

web: &app_base
  image: xhamir/debian-base-app:mri-2.1.6
  ports:
    - "5000:5000" # Bind local port 5000 to App web HTTP port 5000
    - "8990:8989" # Bind local port 8990 to App web debug port 8989
  command: rails server -p 5000 -b 0.0.0.0
  volumes:
    - ncontent-rails:/app
  ######################################
# With linked containers, Docker writes entries to the container's /etc/hosts.
  # We'll try here naming the entries docker will insert into the container's /etc/hosts,
  # so we can use more familiar URL's for our app: (See this container's environment section below)
  links:
    - elasticsearch:elasticsearch.ncontent.net
    - mongodb:mongodb.ncontent.net
    - redis:redis.ncontent.net
  environment: &app_environment

    # URL del servicio de MongoDB local:
    MONGOHQ_URL: mongodb://mongodb.ncontent.net:27017/ecm_development

    # URL del servicio de Elasticsearch local:
    ELASTICSEARCH_URL: http://elasticsearch.ncontent.net:9200

    # URL del servicio de Redis local:
    REDIS_URL: redis://redis.ncontent.net:6379

    SITE_URL: http://localhost:5000

    # Nombre del ambiente de la app (development):
    RACK_ENV: development
    RAILS_ENV: development

    VIRUS_TOTAL_NOTIFY_URL: /scan_notifications/

    # Habilitar el server de debug remoto:
    ENABLE_DEBUG_SERVER: true
    RUBY_DEBUG_PORT: 8989
  # Se puede declarar un archivo con variables de entorno:
  env_file:
    - .env
```
Comandos Docker-Compose:
```
# Levantar todos los procesos:
docker-compose up -d # -d para daemonizar

# Parar todos los procesos levantados:
docker-compose stop

# Ver los logs de un proceso en particular (Ej. 'web'):
docker-compose logs web

# Ver los logs de todos los procesos:
docker-compose logs

# Reiniciar un proceso - debe de estar levantado (Ej. 'web'):
docker-compose restart web

# Parar un proceso en particular (Ej. 'web'):
docker-compose stop web

# Levantar una consola de rails de la app (Ej. 'web'):
docker-compose run web rails c

```

### **Docker-Machine + Docker-Swarm**

Es un cluster nativo de docker que convierte los hosts de Docker en uno solo virtual.


Paso 1: En una VM correr:
> ```
> docker run swarm create
1257e0f0bbb499b5cd04b4c9bdb2dab3 #Token
>```
> Esto creara el token del cluster.

Paso 2: Swarm Master:
> ```
> docker-machine create \
    -d virtualbox \
    --swarm \
    --swarm-master \
    --swarm-discovery token://<TOKEN-FROM-ABOVE> \
    swarm-master 
>```

Paso 3: Swarm Nodes:
>```
>docker-machine create \
    -d virtualbox \
    --swarm \
    --swarm-discovery token://<TOKEN-FROM-ABOVE> \
    swarm-node-00 
> ```
> Se pueden crear tantos nodos como sean necesarios.

Para poder entrar al swarm master 
```
eval "$(docker-machine env --swarm swarm-master)"
```

Entrando al swarm master puedes crear tus contenedores pero la función de build para crear una imagen desde un Dockerfile no esta disponible aún.

Ejemplo:
```
docker run -d -p 80:80 --name frontend nginx
docker run -d --name logger -e affinity:container==frontend logger
```

Estos comandos crean dos contenedores corriendo en la misma red mismo nodo.

-----
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED                  STATUS              PORTS                           NODE        NAMES
87c4376856a8        nginx:latest        "nginx"             Less than a second ago   running             192.168.0.42:80->80/tcp         node-1      frontend
963841b138d8        logger:latest       "logger"            Less than a second ago   running                                             node-1      logger

```
-----
Los docker swarm tienen filtros,  el filtro de afinidad se usa para crear la atracción entre contenedores.

La integración de la creación de una app compleja con compose dentro de swarm aún no esta lista ya que los diferentes host de cada nodo no se comunicación en la aplicación 

Se puede hacer un docker-compose en cada nodo y en el master pero sin existir una comunicación entre ellos.
