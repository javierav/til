# Certificados SSL Autofirmados

Existen dos tipos de certificados SSL:

1. Aquellos que son generados por una entidad certificadora reconocida internacionalmente (y que
nos cobra por el proceso).

2. Aquellos que son generados por nosotros mismos (y gratis).

A estos últimos se les conoce como certificados autofirmados, ya que nadie tiene que verificar la
autenticidad de los datos del certificado. Pero esto supone un problema, ya que el navegador a
priori no tiene forma de comprobar su validez y por tanto nos mostrará el tipico aviso de seguridad.

La solución a este problema es ¡crear nuestra propia entidad certificadora! De esta manera,
instalando el certificado público de nuestra entidad en aquellos navegadores que vayan a usar
nuestras webs seguras, podrás acceder a las mismas sin que se muestre el aviso de seguridad. La
única diferencia respecto a las entidades internacionales es que ellas ya tienen sus certificados
instalados en la mayoría de sistemas operativos, con lo que el proceso es transparente al usuario.
En nuestro caso esa opción no es posible, por lo que tenemos que instalarlo a mano. Evidentemente
esta solución solo es útil en entornos controlados como los de una empresa, nuestra propia casa o
el ordenador de trabajo.


## Certificado CA

El primer paso es generar la clave privada para el certificado raíz de la Entidad Certificadora
(CA) que va a ser la encargada de firmar (dar veracidad) a todos nuestros certificados.

```
$ openssl genrsa -out rootCA.key 2048
```

Con esta llave ya podemos generar el certificado propiamente dicho:

```
$ openssl req -x509 -new -nodes -key rootCA.key -days 365 -out rootCA.crt
```

Como se puede apreciar, al certificado raíz de nuestra CA le hemos puesto un periodo de validez de
un año, tras lo cual tendremos que volver a generar otro certificado y volver a generar todos los
certificados firmados con esta CA, por lo que quizás resulte más práctico ampliar ese periodo a 5 o
10 años.

Nuestra CA se compone de dos archivos:

1. `rootCA.key` contiene la clave privada del certificado.

2. `rootCA.crt` contiene el certificado (clave pública). Este archivo es el que debemos importar a
todos los navegadores que queremos que acepten nuestros certificados. Incluso de puede distribuir
libremente a través de internet. ¡Es la clave pública!


# Certificados adicionales

Una vez que tenemos generado el certificado de nuestra propia CA, llega el turno de generar el
primer certificado firmado por ella. Este paso habrá que repetirlo cada vez que queramos generar un
nuevo certificado SSL, por ejemplo para una nueva web interna.

El primer paso es generar un clave privada:

```
$ openssl genrsa -out host.key 2048
```

**Nota**: Tenemos que generar una clave privada distinta para cada nuevo certificado.

El siguiente paso es generar una solicitud de firma de certificado (CSR). Esto es necesario ya que
en teoría la entidad certificadora no tiene por qué tener acceso a nuestra clave privada, por lo
que le enviamos una petición de firma usando nuestra clave privada y de esa manera la CA puede
generarnos un certificado firmado.

```
$ openssl req -new -key host.key -out host.csr
```

Al ejecutar ese comando nos va a pedir que introduzcamos información sobre el certificado. Este
paso es importante ya que aquí es dónde debemos especificar el dominio para el cual queremos
generar el certificado en el caso de que lo estemos generando para un sitio web.

```
Country Name (2 letter code) [AU]:ES
State or Province Name (full name) [Some-State]:Cordoba
Locality Name (eg, city) []:Cordoba
Organization Name (eg, company) [Internet Widgits Pty Ltd]:NoSoloSoftware Network S.L.
Organizational Unit Name (eg, section) []:ICT
Common Name (e.g. server FQDN or YOUR name) []:nosolosoftware.es
Email Address []:hola@nosolosoftware.es

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

Como se puede apreciar nos pide una serie de datos como el nombre de la empresa, su correo, país y
provincia, aunque el más importante es el `Common Name`, dónde debemos poner el dominio.
Opcionalmente se puede especificar una contraseña que habrá que introducir cada vez que se haga uso
del certificado. Si se va a usar para una web lo normal es dejarlo en blanco ya que de lo contrario
necesitaremos introducir la contraseña del mismo cada vez que se inicie el mismo, con lo cual puede
resultar del todo impráctico. Eso si, debemos asegurarnos de establecer los permisos sobre los
certificados de manera que sólo el usuario del servidor tenga acceso a ellos.

El último paso consiste en usar esa solicitud de firma para generar el certificado con la clave
privada y el certificado de nuestra CA:

```
$ openssl x509 -req -in host.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out host.crt -days 365
```

Nuevamente hemos puesto la duración del certificado a un año, pero se puede cambiar por cualquier
otra fecha. Tras este paso dispondremos de los siguientes archivos:

1. `host.key` la clave privada de este certificado. Hay que subirla al servidor web y mantenerla en
secreto.

2. `host.crt` el certificado público. Se puede distribuir libremente y también hay que subirlo al
servidor web.

3. `host.csr` el archivo de la petición. Una vez completada la generación del certificado ya no
sirve para nada, se puede borrar.
