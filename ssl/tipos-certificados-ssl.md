# Tipos de certificados SSL

## Funcionamiento

Gracias a los certificados SSL podemos transmitir información de forma segura, cifrando y
descifrando los datos transmitidos entre ambas partes, pero ¿cómo funciona un certificado SSL
exactamente?

El funcionamiento es transparente para el visitante que accede a nuestro sitio web, debido a que el
navegador ya cuenta con la información necesaria para interpretar la información transmitida
mediante el certificado SSL instalado en el servidor Web que visita.

Los pasos realmente son:

1. El navegador accede a la web y comprueba el certificado para asegurarse de que conecta al sitio
real y no a uno que pueda estar interceptando esta información.

2. Determina el tipo de cifrado que el navegador y servidor pueden utilizar para "entenderse" entre
ellos.

3. El navegador del visitante y el servidor web intercambian códigos únicos que utilizarán a la
hora de cifrar o descifrar la información que se transmiten entre ellos.

4. El navegador y el servidor web empiezan a utilizar el cifrado, a partir de este momento ya
aparece el candado (o también la barra verde en el navegador, dependiendo del certificado
utilizado) y las páginas e información son transmitidos de forma segura.


## Tipos de validación de los certificados

Cuando adquirimos un certificado SSL podemos elegir entre tres tipos de validación que la empresa
certificadora proporciona.

### Validación de Dominio (DV)

Los certificados SSL de validación de dominio cuentan con la emisión más rápida, pues solamente
precisan de la verificación del dominio (mediante un correo enviado a tu e-mail) para completar el
procedimiento. De esta manera podrás obtener tu certificado SSL a un precio muy bajo y en un tiempo
récord.

### Validación de Empresa (OV)

Los certificados SSL de validación por empresa precisan de datos adicionales para completar su
emisión, para ello será necesario validar el dominio mediante un correo electrónico, presentar
documentos de empresa y responder a una llamada telefónica que realizará la empresa encargada de la
emisión del certificado SSL contratado.

### Validación Extendida (EV)

Los certificados SSL de Validación Extendida o Extended Validation (EV) proporcionan un alto grado
de confianza a tus visitantes gracias a que mostrarán la barra verde en el momento de acceder a tu
sitio web. Este tipo de certificados precisan además de la verificación de datos del dominio y la
empresa, del envío de formularios firmados para poder obtenerla.


## La barra verde

La barra verde es sin lugar a dudas uno de los signos distintivos de los certificados SSL más
conocidos, de un vistazo tus visitantes podrán visualizar la información sobre la empresa a la que
pertenece la web, además del candado habitual que aparece en el resto de certificados SSL. Gracias
a ello, la confianza de tus posibles clientes se verá reforzada en el momento de introducir
información personal o incluso financiera.

> **Importante:** Esta funcionalidad únicamente está disponible en los certificados de validación
> extendida.

_Figura 1: Ejemplo de la barra de navegación en certificados de validación de **dominio** y
**empresa**._

![Barra de navegación simple](images/green-bar-simple.png)

_Figura 2: Ejemplo de la barra de navegación en certificados de validación **extendida**._

![Barra de navegación verde](images/green-bar-extended.png)


## Dominios de los certificados

En función del número de dominios sobre los que se quiera aplicar el certificado existen los
siguientes tipos:

### Dominio único

Los certificados de dominio único te permiten añadir seguridad SSL a un único dominio, por este
motivo suelen tener un precio más ajustado que los certificados de otro tipo. Si dispones de un
único proyecto web que necesita disponer de Seguridad SSL éstos serán tu mejor opción.

### Múltiples dominios

Los certificados multidominio te permitirán añadir seguridad SSL a múltiples dominios mediante un
único certificado SSL, se trata de la opción más cómoda, de esta manera podrás añadir un
certificado SSL a otro dominio con facilidad. Suelen permitir entre 3 y 5 dominios.

### Wildcard

Los certificados SSL Wildcard te permitirán añadir seguridad SSL a todos los subdominios de tu
dominio mediante un único certificado SSL, de esta manera, si tienes distintos subdominios podrán
hacer uso del mismo certificado sin coste adicional.

> **Importante:** este tipo de certificado es incompatible con la validación extendida.


## Documentación requerida

En los certificados de validación de empresa y validación extendida es necesario adjuntar una serie
de documentos que identifican a la empresa y que son requeridos por las autoridades certificadoras
con el fin de poder validar y por tanto certificar que el propietario del dominio es quién dice
ser.

### Validación de dominio

La Validación de Dominio o Domain Validation (DEV) es la verificación más rápida a la hora de
generar un certificado SSL, debido a la sencillez de esta verificación podrás disponer de tu
certificado SSL en pocos minutos. Para llevar a cabo la verificación bastará con responder a un
correo electrónico que es enviado de forma automatizada a tu cuenta de correo.

> **Importante**: No es posible validar dominios con privacidad Whois activada, esto es debido a
> que el correo es enviado a la cuenta de correo que aparece en el Whois. En caso de utilizar un
> servicio de Whois Privado tendrás que desactivarlo temporalmente al efectuar tu solicitud.

### Validación de empresa

La Validación de Empresa o Business Validation (BV) consiste en, además de validar el dominio,
enviar también datos relativos a tu empresa.

1. **Validación del dominio**

	Se trata del paso más sencillo, pues simplemente tendrás que responder a un mensaje automatizado
  que será enviado a tu cuenta de correo electrónico.

2. **Envío de documentación de la empresa**

	Deberás proporcionar documentación relativa a tu empresa a la empresa certificadora, ya sea por
  e-mail (en formato PDF), vía fax o incluso por vía postal. Tendrás que proporcionar uno de los
  siguientes documentos: Documento de incorporación de la empresa (CIF) o la licencia de apertura.

3. **Verificación mediante llamada telefónica**

	Para completar la verificación, la entidad encargada de ella procederá a comprobar la veracidad
  de los datos mediante una llamada telefónica, para ello comprobarán el número telefónico mediante
  bases de datos públicas.

### Validación extendida

La Validación Extendida o Extended Validation (EV) es la verificación más completa de las
disponibles, pues además de validar el dominio y la empresa deberás enviar documentación adicional
que la empresa certificadora (Issuer) revisará detenidamente. Recomendamos el uso de certificados
SSL de Validación Extendida a tiendas online y empresas que cuentan con carritos de compra, debido
a que dispondrás de la Barra verde, la cual aparecerá en el navegador de tus visitantes, y es el
signo más visible del tipo de seguridad del que cuenta tu sitio web, lo que aumentará la confianza
de tus posibles clientes a la hora de introducir información sensible en él.

1. **Validación del dominio**

	Se trata del paso más sencillo, pues simplemente tendrás que responder a un mensaje automatizado
  que será enviado a tu cuenta de correo electrónico.

2. **Envío de documentación de la empresa**

	Deberás proporcionar documentación relativa a tu empresa a la empresa certificadora, ya sea por
  e-mail (en formato PDF), vía fax o incluso por vía postal. Tendrás que proporcionar uno de los
  siguientes documentos: Documento de incorporación de la empresa (CIF) o la licencia de apertura.
	
3. **Envío de los documentos EV**

	Una vez enviados los datos relativos a tu empresa, deberás enviar también los formularios de
  solicitud de validación EV a la empresa certificadora. Este proceso es más lento, pero te
  permitirá obtener la máxima verificación disponible. Documentos: Formulario de petición de
  certificado y Acuerdo de suscriptor de Certificado SSL EV.

4. **Verificación mediante llamada telefónica**

	Para completar la verificación, la entidad encargada de ella procederá a comprobar la veracidad
  de los datos mediante una llamada telefónica, para ello comprobarán el número telefónico mediante
  bases de datos públicas.

> **NOTA**: Toda la información de esta página ha sido obtenida del portal web de
> [*Don Dominio*](http://dondominio.com), por tanto no se aplica la licencia general de este
> repositorio.

