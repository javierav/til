# Introducción a Chef Solo

[**Chef**](https://www.chef.io/chef/) es un proyecto de código abierto escrito en *Ruby* para la
gestión de la configuración de un servidor. Permite a los administradores de sistemas definir cómo
debe estar configurado un equipo y posteriormente aplicar esa configuración a una o varias
máquinas. De esta forma se queda documentado todo el proceso que hay que seguir para dejar el
servidor como debe estar y nos permite restaurar la configuración de manera automática después de
un desastre o si añadimos nuevas máquinas.

*Chef* tiene dos modos de trabajo:

1. Usando *Chef Server*, dónde la configuración se sube a un servidor remoto y cada una de las
máquinas tienen un cliente instalado que se comunica con el servidor de *Chef* para recibir las
tareas que tiene que ejecutar.

2. Usando *Chef Solo*, dónde la configuración se almacena directamente en la máquina que vamos a
configurar y mediante un comando se interpreta y se ejecuta. Nosotros debemos ser los responsables
de copiar las recetas a cada servidor.

Cada uno de los modos tiene su
[ventajas e inconvenientes](http://serverfault.com/questions/514104/chef-server-vs-chef-solo) y
depende mucho de las necesidades de tu infraestructura.

> Nota: no es el objetivo de esta guía definir extensamente qué es *Chef* y las distintas partes de
> su arquitectura, dejando al lector la responsabilidad de leer documentación externa para
> comprender el alcance del proyecto.


## Knife Solo

Existe un plugin de *Knife* (una de las herramientas de *Chef*) llamado Knife Solo que nos permite
ejecutar en un servidor remoto las recetas que tenemos definidas en nuestro ordenador sin necesidad
de tener que copiarlas a mano en cada una de las máquinas que deseamos configurar.

Este plugin resulta muy interesante cuando tenemos pocas máquinas y no queremos complicarnos la
vida con una instalación de *Chef Server* o usar *Chef Solo* a pelo, como es mi caso. A
continuación vamos a ver cómo se usa mediante un ejemplo muy sencillo: queremos instalar un
servidor de *Redis*, una base de datos de clave-valor muy popular.


## Instalación

Como con casi todo lo que rodea a *Chef*, necesitamos tener instalado *Ruby* en nuestro sistema, ya
sea mediante un gestor de paquetes como *DEB*, *RPM* o *Homebrew* o mediante un sistema de
versiones como *RVM*.

> Nota: se da por hecho que el usario tiene conocimientos basicos de *Ruby* y su ecosistema.

Procedemos a instalar las gemas necesarias:

```
$ gem install chef knife-solo librarian-chef --no-ri --no-rdoc
```


## Inicialización

En *Chef* se trabaja por respositorios de código que contienen las recetas que se pueden aplicar a
cada una de las máquinas que tenemos. Podemos tener un solo repositorio con todas las recetas
usadas en alguna máquina o podemos tener un repositorio separado para cada una, eso queda a
elección del administrador. Mi opinión es que usar distintos repositorios acaba duplicando código y
dificultando su gestión, aunque puede haber casos en los que esto no sea del todo cierto. En este
tutorial vamos a usar un solo repositorio dada la simpleza del ejemplo.

Inicializamos el repositorio con el comando `init`:

```
$ knife solo init .

WARNING: No knife configuration file found
Creating kitchen...
Creating knife.rb in kitchen...
Creating cupboards...
Setting up Librarian...
```

Si hacemos un `ls -l` podemos ver la estructura de carpetas y archivos recién creada.

```
$ ls -l

-rw-rw-r-- 1 jaranda jaranda   45 feb  5 12:01 Cheffile
drwxrwxr-x 2 jaranda jaranda 4,0K feb  5 12:01 cookbooks
drwxrwxr-x 2 jaranda jaranda 4,0K feb  5 12:01 data_bags
drwxrwxr-x 2 jaranda jaranda 4,0K feb  5 12:01 environments
-rw-rw-r-- 1 jaranda jaranda   80 feb  5 11:30 Gemfile
-rw-rw-r-- 1 jaranda jaranda 3,4K feb  5 11:36 Gemfile.lock
drwxrwxr-x 2 jaranda jaranda 4,0K feb  5 12:01 nodes
-rw-rw-r-- 1 jaranda jaranda  913 feb  5 12:01 README.md
drwxrwxr-x 2 jaranda jaranda 4,0K feb  5 12:01 roles
drwxrwxr-x 2 jaranda jaranda 4,0K feb  5 12:01 site-cookbooks
```

Veamos para qué sirve cada elemento:

* **.chef** - contiene la configuración básica de knife.rb
* **Cheffile** - archivo de configuración de la herramienta Chef-Librarian
* **cookbooks** -directorio dónde se almacenarán todas las recetas descargadas mediante Librarian.
* **data_bags** - aquí se encuentras los archivos de las *data bags*.
* **environments** - configuración de los entornos (opcional).
* **nodes** - configuración de los nodos (máquinas físicas o virtuales) creadas automáticamente por
  knife-solo.
* **roles** - configuración de los roles (opcional).
* **site-cookbooks** - directorio para almacenar las recetas creadas por nosotros mismos.


## Preparación del servidor

Como ya hemos visto antes, *Chef* Solo ejecuta las recetas en el mismo equipo dónde esté instalado.
El plugin *Knife Solo* que estamos usando es una herramienta que automatiza el proceso de subir las
recetas al servidor y ejecutarlas, pero sigue necesitando de la instalación de *Chef* Solo en el
servidor.

Para no tener que hacer este proceso a mano, el plugin incorpora una utilidad para hacer esto más
automático, y debemos ejecutarla cada vez vayamos a provisionar un servidor la primera vez.

```
$ knife solo prepare production01

Bootstrapping Chef...
Getting information for chef stable 12.6.0 for ubuntu...
Comparing checksum with sha256sum...
Installing chef 12.6.0
installing with dpkg...
Seleccionando el paquete chef previamente no seleccionado.
(Leyendo la base de datos ... 50936 ficheros o directorios instalados actualmente.)
Preparing to unpack .../chef_12.6.0-1_amd64.deb ...
Unpacking chef (12.6.0-1) ...
Configurando chef (12.6.0-1) ...
Thank you for installing Chef!
Generating node config 'nodes/production01.json'...
```

En este ejemplo, `production01` es un host que tenemos añadido al archivo de configuración del
cliente de *SSH* situado en `~/.ssh/config`

```
host production01
  hostname <ip>
  user root
  identityfile ~/.ssh/<id>
```

Personalmente en lugar de usar una dirección IP o un dominio que apunte a nuestro servidor, me
gusta usar este tipo de hosts ya que son más fáciles de recordar. Sobre gustos...


## Añadir una receta

En *Chef* podemos escribir nuestras propias recetas o usar alguna ya creada por la comunidad. En
nuestro caso vamos a usar la herramienta *Librarian* ya instalada para bajarnos una receta creada
por la comunidad que instale un servidor de *Redis*. Esta herramienta funciona de forma muy similar
a cómo funciona *Bundler* para *Ruby*: lee un archivo de configuración para saber cuales son las
recetas que debe instalar en nuestro repo. Para ello debemos configurar el archivo `Cheffile` y
añadir en él la receta que queremos usar:

```
site 'http://supermarket.getchef.com/api/v1'

cookbook 'redisio', '~> 2.3.0'
```

El siguiente paso es descargar la receta que acabamos de indicar mediante el correspondiente
comando:

```
$ librarian-chef install

Installing build-essential (2.2.4)
Installing ulimit (0.3.3)
Installing redisio (2.3.0)
```

Se ha descargado la receta indicada así como aquellas dependencias que tuviera la misma.


## Instalar una receta en un servidor

Una vez tenemos las recetas que vamos a necesitar, el siguiente paso es indicarle a *Chef* qué debe
instalar en qué servidor y con qué configuración. Eso se hace editando el archivo correspondiente
al host que queremos configurar que se encuentra en la carpeta *nodes*.

En nuestro ejemplo editamos el archivo `nodes/production01.json` para añadirle en el *runlist* la
nueva receta que tiene que ejecutar:

```
{
  "run_list": [
    "recipe[redisio]",
    "recipe[redisio::enable]"
  ]
}
```

Cuando hayamos terminado de definir la configuración, procedemos a ejecutar la receta en el
servidor con el comando:

```
$ knife solo cook production01
```

Este comando se encarga de subir las recetas de nuestro repositorio al servidor indicado y
ejecutarlas mediante la instalación de *Chef Solo* que tiene y devolvernos los resultados por
nuestra pantalla. ¡Una maravilla!


