**1. ACTUALIZACIÓN DE LA MÁQUINA**

Primero, actualiza los repositorios e instala las últimas actualizaciones disponibles:

*(sudo apt update)*

![INSTALACION](unnamed.png)

*(sudo apt upgrade)*

![INSTALACION](unnamed(1).png)


**2. INSTALACIÓN DEL SERVIDOR WEB APACHE2**

Para instalar el servidor web Apache, utiliza el siguiente comando:

*(sudo apt install -y apache2)*

![INSTALACION](unnamed(2).png)


**3. INSTALACIÓN DEL SERVIDOR DE BASES DE DATOS MYSQL**

Instalar MySQL Server:

*(sudo apt install -y mysql-server)*

![INSTALACION](unnamed(3).png)


**4. INSTALACIÓN DE PHP Y BIBLIOTECAS NECESARIAS**

A continuación, instala PHP y las bibliotecas necesarias para integrarlo con Apache y MySQL:

*(sudo apt install -y php libapache2-mod-php)*

![INSTALACION](unnamed(4).png)

*(sudo apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl)*

![INSTALACION](unnamed(5).png)

**5. REINICIA APACHE PARA QUE LOS MODULOS DE PHP SE CARGUEN CORRECTAMENTE:**

*(sudo systemctl restart apache2)*

![INSTALACION](unnamed(6).png)

**6. AHORA TIENES QUE CONFIGURARLO**
