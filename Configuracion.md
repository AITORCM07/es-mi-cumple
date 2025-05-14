**1. Configuración de MySQL**
Accede a la consola de MySQL:

*sudo mysql*

![INSTALACION](fotito1.png)


**1.1. Creación de la base de datos**
Desde la consola de MySQL, crea una base de datos llamada bbdd (o el nombre que prefieras):

*CREATE DATABASE bbdd;*

![INSTALACION](fotito2.png)


**1.2. Creación de un usuario MySQL**
A continuación, crea un usuario con acceso a la base de datos bbdd. El siguiente comando crea un usuario llamado usuario con la contraseña password, y le da acceso solo desde localhost:

*CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';*

![INSTALACION](fotito3.png)


**1.3. Otorgar privilegios al usuario**
Para otorgar privilegios completos al usuario para trabajar con la base de datos bbdd, ejecuta el siguiente comando:

*GRANT ALL ON bbdd. TO 'usuario'@'localhost';*

![INSTALACION](fotito4.png)


Finalmente, sal de la consola de MySQL:

*exit*

![INSTALACION](fotito5.png)


**1.4. Comprobar conexión**
Prueba que puedes conectarte a MySQL con el usuario y la contraseña creados:

*mysql -u usuario -p*

![INSTALACION](fotito6.png)

**____________________________________________________________________________________________________________________________________________________**

**2. Modificar archivo de configuración de MySQL**
Edita el archivo de configuración de MySQL para permitir conexiones desde cualquier IP (o desde una IP específica si es necesario). Abre el archivo mysqld.cnf con un editor de texto, por ejemplo, vim:

*sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf*

Busca la línea bind-address y cámbiala a 0.0.0.0 para permitir conexiones desde cualquier máquina:

*bind-address = 0.0.0.0*

Guarda los cambios y cierra el editor.


**2.1. Reiniciar MySQL**
Para aplicar los cambios, reinicia el servicio de MySQL:

*sudo systemctl restart mysql*

**____________________________________________________________________________________________________________________________________________________**

**3. Creación de un usuario para acceso remoto**
Ahora, para permitir que un usuario se conecte desde una máquina remota (por ejemplo, desde la IP 192.168.22.100), crea un nuevo usuario y asigna privilegios adecuados:

*CREATE USER 'usuario'@'192.168.22.100' IDENTIFIED WITH mysql_native_password BY 'password';*

Luego, otorga privilegios al nuevo usuario sobre la base de datos bbdd:

*GRANT ALL ON bbdd.* TO 'usuario'@'192.168.22.100';*

Finalmente, sal de la consola de MySQL:

exit

**____________________________________________________________________________________________________________________________________________________**

**4. Verificación de la conexión remota**
Desde una máquina remota, intenta conectarte a MySQL con el usuario y la contraseña creados:

*mysql -u usuario -p -h <IP_DEL_SERVIDOR_MYSQL>*

**____________________________________________________________________________________________________________________________________________________**

**5. Configuras el owncloud** 


**5.1 Actualizar las listas de paquetes e instalar las actualizaciones disponibles**
Lo primero es asegurarse de que tu sistema esté completamente actualizado. Ejecuta los siguientes comandos para actualizar las listas de paquetes e instalar las actualizaciones:


*sudo apt update*
*sudo apt upgrade -y*


**5.2 Instalar los requisitos previos del PPA**
Para trabajar con paquetes de un PPA (repositorios personales de aplicaciones), necesitamos instalar un paquete adicional. Este paquete es software-properties-common, que permite gestionar las fuentes de software adicionales. Instálalo con:


*sudo apt install software-properties-common -y*


**5.3. Agregar el PPA para PHP (Ondřej Surý's PHP PPA)**
El siguiente paso es agregar el repositorio que contiene las últimas versiones de PHP. El PPA de Ondřej Surý es uno de los más utilizados para obtener versiones recientes de PHP en distribuciones basadas en Debian/Ubuntu.

Ejecuta el siguiente comando para agregar el PPA de PHP:


*LC_ALL=C.UTF-8 sudo add-apt-repository ppa:ondrej/php -y*

Este comando agrega el PPA a las fuentes de software de tu sistema.


**5.4 Actualizar los repositorios**
Una vez que se ha agregado el PPA, debes actualizar nuevamente los repositorios para que el sistema reconozca los paquetes disponibles en este nuevo repositorio:


*sudo apt update*


**5.5 Instalar PHP 7.4 y las extensiones necesarias**
Ahora que hemos configurado los repositorios, podemos proceder con la instalación de PHP 7.4 y las extensiones necesarias para que funcione correctamente en el servidor web Apache.

Instalar PHP 7.4:

*sudo apt install php7.4 -y*

Instalar el módulo de PHP para Apache (mod_php):

*sudo apt install -y php libapache2-mod-php7.4*

Instalar las extensiones de PHP 7.4 necesarias para diversas funciones:

*sudo apt install -y php7.4-fpm php7.4-common php7.4-mbstring php7.4-xmlrpc php7.4-soap php7.4-gd php7.4-xml php7.4-intl php7.4-mysql php7.4-cli php7.4-ldap php7.4-zip php7.4-curl*

Estas extensiones cubren una variedad de funcionalidades útiles, como soporte para bases de datos MySQL, procesamiento de imágenes, manejo de XML, etc.


**5.6 Seleccionar la versión de PHP por defecto**
Si tienes varias versiones de PHP instaladas en tu servidor, puedes seleccionar cuál quieres usar como la versión predeterminada. Para ello, usa el siguiente comando:

*sudo update-alternatives --config php*

Este comando te presentará una lista de versiones de PHP disponibles en tu sistema. Ingresa el número correspondiente a la versión de PHP que quieres establecer como predeterminada (en este caso, PHP 7.4).


**5.7 Activar los módulos de Apache necesarios para PHP 7.4**
Para que Apache maneje correctamente las solicitudes de PHP 7.4, debes habilitar algunos módulos específicos y configuraciones. Los módulos necesarios son:

Activar los módulos proxy_fcgi y setenvif:

*sudo a2enmod proxy_fcgi setenvif*

Activar la configuración de php7.4-fpm (FastCGI Process Manager):

*sudo a2enconf php7.4-fpm*


**5.8 Reiniciar Apache2**
Después de realizar todos los cambios, es necesario reiniciar Apache para que los nuevos módulos y configuraciones surtan efecto. Ejecuta el siguiente comando para reiniciar Apache:

*sudo service apache2 restart*

**____________________________________________________________________________________________________________________________________________________**

**6. Luego copias los archivos de la aplicación web al servidor**
Primero, asegurémonos de tener el archivo comprimido de la aplicación web en una ubicación accesible. En este ejemplo, se ha descargado el archivo app-web.zip en el directorio ~/Baixades (el directorio "Descargas" en catalán). Si el idioma de tu sistema es diferente, deberás adaptar el comando de acuerdo con la ruta en tu idioma.


**6.1. Copiar el archivo al directorio de Apache**
Para copiar el archivo comprimido de la aplicación web al directorio raíz de Apache (/var/www/html), utiliza el siguiente comando:

*sudo cp ~/Baixades/app-web.zip /var/www/html*
Nota: Si la carpeta "Baixades" no existe, debes modificar el comando para reflejar la ruta correcta donde descargaste el archivo ZIP.


**6.2. Acceder al directorio de Apache**
Luego, accedemos al directorio donde vamos a trabajar, que es /var/www/html, con el siguiente comando:

*cd /var/www/html*


**6.3. Descomprimir el archivo ZIP**
Descomprime el archivo ZIP que contiene la aplicación web con el siguiente comando:

*sudo unzip app-web.zip*

Esto descomprimirá el contenido del archivo ZIP en el directorio actual (/var/www/html).


**6.4. Mover los archivos a la ubicación adecuada**
A continuación, movemos el contenido descomprimido de la carpeta app-web a la raíz de /var/www/html. El nombre de la carpeta descomprimida puede variar, por lo que debes adaptarlo a la carpeta que se haya creado al descomprimir el archivo.

*sudo cp -R app-web/. /var/www/html*


**6.5. Eliminar la carpeta descomprimida**
Una vez que los archivos hayan sido copiados correctamente, puedes eliminar la carpeta descomprimida (app-web) para mantener tu sistema limpio:

*sudo rm -rf app-web/*


**6.6. Eliminar el archivo index.html predeterminado de Apache**
El archivo index.html que viene por defecto con Apache a veces puede interferir con el acceso a tu aplicación web. Por eso, eliminamos este archivo para evitar conflictos:

*sudo rm -rf /var/www/html/index.html*

**____________________________________________________________________________________________________________________________________________________**

**7. Configurar permisos de los archivos de la aplicación web**
Ahora que los archivos de la aplicación web están en el directorio /var/www/html, es necesario configurar los permisos adecuados para que Apache pueda acceder a ellos sin problemas.


**7.1. Establecer permisos correctos**
Establece los permisos de los archivos a 775 para permitir la escritura, lectura y ejecución a los propietarios y al grupo (en este caso, Apache):

*cd /var/www/html*
*sudo chmod -R 775 .*


**7.2. Cambiar propietario y grupo**
Cambia el propietario de los archivos a usuario y el grupo a www-data, que es el grupo usado por Apache en la mayoría de los sistemas Linux:

*sudo chown -R usuario:www-data .*

Esto asegura que Apache tenga los permisos necesarios para servir los archivos y que el usuario usuario (el que tiene acceso a la base de datos) pueda manipular los archivos si es necesario.

**____________________________________________________________________________________________________________________________________________________**

**8. Configurar la base de datos en la aplicación web**
Durante la instalación, la aplicación web te pedirá que introduzcas los detalles de la base de datos. Asegúrate de proporcionar la siguiente información:

*Usuario de la base de datos: usuario*
*Contraseña de la base de datos: password*
*Base de datos: bbdd*
*Dominio: localhost*
*Con esta información, la aplicación podrá conectarse correctamente a la base de datos MySQL que configuramos anteriormente.*

____________________________________________________________________________________________________________________________________________________

**9. Crear un usuario administrador**
En la siguiente pantalla, el instalador de la aplicación web te pedirá que crees un usuario administrador para acceder a la aplicación. Completa los campos con el nombre de usuario y la contraseña que desees, y guarda los cambios.

____________________________________________________________________________________________________________________________________________________

**10. Finalizar la instalación**
Una vez que hayas configurado la base de datos y creado un usuario administrador, la instalación de la aplicación web debería completarse. Ahora puedes acceder a la aplicación en cualquier momento usando la URL:

*http://localhost*

