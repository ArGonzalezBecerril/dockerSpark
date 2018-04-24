[![N|Solid](https://image.ibb.co/iBcbcS/1_titulo_Documento.png)](https://nodesource.com/products/nsolid)

### ¿Que es docker?
Es una plataforma de software que nos permite crear, probar e implementar aplicaciones rápidamente ya que empaqueta software en unidades estandarizadas llamadas contenedores.

### 1- Herramientas necesarias. 
  - SO linux debian 7
  - Ultima versión de docker
  - Contenedor de spark

#### 1.1 - Instalación de docker
Para este primera parte se instalara docker en debian 7 o derivados(Linux mint 15 ó 16 ó Ubuntu cualquier versión a partir de la versión 12

Para ello se debe abrir una terminar y escribir lo siguiente.

```sh
#Actualizar nuestro repositorio.
arturo@arturo$ sudo apt-get update 
#Agregamos los certificados de seguridad
arturo@arturo$ sudo apt-get install apt-transport-https ca-certificates -y
#Agregar el repositorio docker a nuestro source.list
arturo@arturo$ sudo sh -c "echo deb https://apt.dockerproject.org/repo debian-jessie main > /etc/apt/sources.list.d/docker.list"
#Agregar la llave publica del repositorio
arturo@arturo$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
#Actualizar la cache de repositorio,"Antes es nesesario hacer un sudo apt-get update"
arturo@arturo$ sudo apt-cache policy docker-engine
#Instalar el core de docker
arturo@arturo$ sudo apt-get install docker-engine -y 
#Iniciar el servicio
arturo@arturo$ sudo service docker start
#Probar que funciona con un "hola mundo"
arturo@arturo$ sudo docker run hello-world
#Asignar permiso a mi usuario para que pueda listar imagenes de contenedores y cargarlas en docker.
arturo@arturo$ chown -R bashroot:bashroot /var/run/docker*
#Reiniciamos el servicio
arturo@arturo$ sudo service docker restart 
```
##### 1.2 - Comandos mas usados en docker
```sh
arturo@arturo$ docker rmi idContenedor #Eliminar un contenedor
arturo@arturo$ docker image ls #Lista todas las imagenes de contenedores que tenemos
arturo@arturo$ docker rm -f $(docker ps -a -q) #Elimina todas las imagenes que tenemos cargadas listas para ejecutar, pero no las elimina a nivel de disco.
arturo@arturo$ docker inspect -f '{{.Name}} - {{-NetworkSettings.IPAddress}}'$(docker ps -aq) #Listar todas las ip asignadas a las imagenes que tenemos en docker
arturo@arturo$ docker load -i /home/arturo/hortonworks.tar.gz #Cargar una imagen de un contenedor
arturo@arturo$ docker exec -i -t idImagen /bin/bash #Obtener una terminal del contenedor
```
#### 2.0 - Cargar la imagen de apache spark en docker.
>Existen 2 formas de agregar la imagen de apache spark a docker, el cual son las siguientes:
- Realizar un pull de la imagen de la siguiente url https://github.com/P7h/p7hb-docker-spark
- Si ya se cuenta con un contenedor en otro equipo es mas sencillo comprimir la imagen en  formato .tar.gz e importarla al nuevo equipo donde se requiere instalarlo.

> En mi caso particular ya lo tengo en otro equipo por lo que solo lo comprimo y lo cargo al contenedor de la siguiente manera:

```sh
#Importar un contenedor de spark comprimido
arturo@arturo$  docker load -i /home/bashroot/Software/Docker_Cotenedores/spark.tar.gz
402964b3d72e: Loading layer [===========================>]  24.72MB/24.72MB
9ab7eda5c826: Loading layer [===========================>]  7.462MB/7.462MB
9752c15164a8: Loading layer [===========================>]  146.3MB/146.3MB
f231cc200afe: Loading layer [===========================>]  1.451MB/1.451MB
3df7be729841: Loading layer [===========================>]  3.584kB/3.584kB
ae150883d6e2: Loading layer [===========================>]  1.536kB/1.536kB
f6b229974fdd: Loading layer [===========================>]  476.8MB/476.8MB
581533427a4f: Loading layer [===========================>]  400.9kB/400.9kB
ddc80708a9de: Loading layer [===========================>]  356.7MB/356.7MB
```

### Iniciar el contenedor de Spark
```sh
#Arrancar la imagen de spark
arturo@arturo$ docker run -it -p 4040:4040 -p 8080:8080 -p 8081:8081 -h spark --name=spark p7hb/docker-spark
#Esto nos devuelve una terminal donde ya podemos iniciar spark en modo shell
bash@root$ spark-shell --total-executor-cores 2 --executor-memory 4g
Spark context Web UI available at http://172.17.0.2:4040
Spark context available as sc (master = local[*], app id = local-1505020317043).
Spark session available as spark.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/ '
   /___/ .__/\_,_/_/ /_/\_\   version 2.2.0
      /_/
         
Using Scala version 2.11.8 (OpenJDK 64-Bit Server VM, Java 1.8.0_131)
Type in expressions to have them evaluated.
Type :help for more information.
$scala>

```
License
----
GNU
