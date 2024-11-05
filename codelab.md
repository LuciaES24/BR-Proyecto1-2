author: Lucía Espinosa Sánchez
summary: Codelab en el que se explica paso a paso cómo bastionar el arranque en Debian
id: 2
categories: GRUB,Debian
environments: Web
status: Published

# Configuración para arranque seguro en Debian

## Introducción
Duration: 00:02:00

En este CodeLab aprenderemos a configurar el arranque seguro en Debian, configurando archivos del GRUB. Para realizar esta configuración en mi caso, utilizaré una máquina virtual de VirtualBox en la cual he instalado Debian previamente.

![Foto máquina virtual](/pictures/VMDebian.png)

¿Estas preparad@ para empezar? ¡Vamos allá!

## Contraseña de arranque
Duration: 00:05:00

Para comenzar la configuración de arranque de Debian crearemos una contraseña de arranque. Para ello abriremos la línea de comandos y ejecutaremos el comando <code>su -</code> para ser usuario root.
A continuación, vamos a editar el archivo de configuración utilizando el comando <code>sudo nano /etc/grub.d/40_custom</code>. Una vez lo editemos deberemos añadir al final las siguientes dos líneas:
```
set superusers=“root”
password root CONTRASEÑA
```
Donde CONTRASEÑA será la que nosotros queramos.

El archivo debería quedar algo parecido al siguiente.

![Foto archivo 40_custom](/pictures/nano40_custom.png)

A continuación guardaremos los cambios pulsando Ctrl+O y cerraremos el archivo con Ctrl+X.

Finalmente tendremos que regenerar el fichero para guardar los cambios ejecutando el comando <code>sudo update-grub</code>.

Ya tendremos nuestra contraseña, en el siguiente apartado veremos cómo cifrarla.

## Cifrado de contraseña
Duration: 00:05:00

En este apartado veremos cómo cifrar la contraseña de arranque que hemos designado en el paso anterior.
Para cifrar la contraseña deberemos ejecutar el comando <code>sudo grub-mkpasswd-pbkdf2</code> y nos devolverá la contraseña cifrada, la cual debemos copiar. Se verá algo como esto:

![Foto archivo 40_custom contraseña cifrada](/pictures/hashPwd.png)

A continuación, volveremos a editar el archivo *40_custom* como en el paso anterior y sustituir la línea en la que pusimos la contraseña por la siguiente:
```
password_pbkdf2 CONTRASEÑA_CIFRADA
```
Donde CONTRASEÑA_CIFRADA será la que hemos copiado previamente al ejecutar el comando de cifrado.

El archivo deberá quedar algo así:
![Foto archivo 40_custom contraseña cifrada](/pictures/nanoHashPwd.png)

Guardaremos de nuevo los cambios y cerraremos el archivo. Una vez hecho deberemos volver a regenerar el archivo con el comando <code>sudo update-grub</code>.

En el siguiente paso trataremos algunos permisos. ¡Seguimos!

## Permisos en archivo de configuración
Duration: 00:05:00

Vamos a modificar los permisos en el archivo de configuración que hemos estado editando previamente para que solo pueda ser editado por el root. Para ello ejecutaremos el comando <code>sudo chmod 700 /etc/grub.d/40_custom</code>.

Finalmente vamos a ver cómo ocultar el menú GRUB 2 al arrancar. Este paso es opcional pero ya falta poco para terminar. ¡Vamos! 

## Ocultar el menú GRUB 2
Duration: 00:05:00

Para ocultar el menú de GRUB 2 al arrancar deberemos modificar el archivo *grub* utilizando el comando <code>nano /etc/default/grub</code> siendo usuario root.
Una vez hemos accedido al archivo deberemos modificar el *GRUB_TIMEOUT* a 0, quedando el archivo de la siguiente manera:

![Foto archivo grub](/pictures/nanoTimeOut.png)

Guardaremos el archivo y saldremos.
Finalmente volveremos a regenerar el archivo con el comando <code>update-grub</code>.

Y... ¡ya habremos terminado la configuración de arranque en Debian!

¡Espero que te haya sido útil!