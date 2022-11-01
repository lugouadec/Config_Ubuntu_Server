## Netplan: Configurar la red en Ubuntu 20.04

Seguramente te habrás dado cuenta, que en las últimas versiones de Ubuntu la configuración de la red ha cambiado. Vamos a ver como configurar la red en Ubuntu 20.04 con la herramienta Netplan.

La verdad es que no soy muy fan de estos cambios. En cualquier caso, ya seas un administrador de GNU/Linux en una empresa o lo utilizas a nivel doméstico, debes conocer los conceptos básicos de la configuración de la red.

Este conocimiento básico implica conocer el nombre de la interfaz, la configuración de la IP actual y el nombre del host. Además, saber cómo cambiar las configuraciones predeterminadas y adaptarlas a nuestras necesidades. Así que vamos al lío.

Lo primero que haremos es conocer la herramienta Netplan, esencial para realizar estos cambios.

# ¿Qué es Netplan? ¿Para qué sirve?

Netplan es una nueva utilidad de configuración de red, de línea de comandos, que se introdujo por primera vez en Ubuntu 17.10, para administrar y configurar los ajustes de red, de forma fácil. Permite configurar al usuario una interfaz de red utilizando el formato YAML. Funciona junto a los daemon de red como NetworkManager y systemd-networkd, como interfaces para el kernel.

Netplan se encarga de leer la configuración indicada en los ficheros de la carpeta /etc/netplan/*.yaml y puede almacenar las configuraciones para todas las interfaces de red en estos archivos.

# ¿Cómo ver la IP actual en Ubuntu 20.04?

Para saber la IP actual de nuestro dispositivo, simplemente debemos escribir:
___   
ip -a
___   
O bien:
___
ip addr
___

# Configurar una IP estática

A partir de esta versión se utiliza la herramienta de administración de red llamada Netplan. Su archivo de configuración se encuentra en el directocio /_etc/netplan_.  Si queremos saber cómo se llama el fichero de configuración de nuestro equipo, solo hemos de lanzar un ls.
___
ls /etc/netplan
___

De esta manera podemos saber el nombre del fichero de configuración, que como se ve en la imagen esta en formato YAML. En mi caso es el fichero _01-network-manager-all.yaml_. Para editarlo podemos utilizar nuestro editor favorito: vi, vim, emacs, nano, etcétera. Aunque lo idea es siempre hacer una copia de seguridad del fichero original, así

___
sudo cp -p /etc/netplan/01-network-manager-all.yaml /etc/netplan/01-network-manager-all.yaml.original
___

## De esta manera ya podemos editarlo:

___
sudo vim /etc/netplan/01-network-manager-all.yaml
___

Aquí podemos ver un ejemplo de configuración para IP estática. Ojo cuidado aquí, ya que los ficheros YAML los carga el diablo, y debemos cumplir con las sangrías correspondientes, sino no nos funcionará. Para aplicar el cambio guardamos y salimos.  Y ejecutamos el siguiente comando de validación de la configuración:
___
sudo netplan try
___
Si nos valida la configuración, recibiremos un mensaje de configuración aceptada; en cambio, si va mal, nos devuelve a la configuración anterior. Para amplicar los cambios:
___
sudo netplan apply
___
Aquí cuidado, que me he vuelto un poco loco porque no me resolvía con el servidor DNS que había configurado. Esto era debido a que el servicio «systemd-resolved» lo tenía parado, para encenderlo y añadirlo al arranque:

___
sudo systemctl start systemd-resolved
sudo systemctl enable systemd-resolved
___
Si no queremos utilizar este servicio, podemos seguir utilizando el fichero de configuración _/etc/resolv.conf_.

Bien, veamos si ha hecho el cambio:

# Configurar la IP dinámica

Esta parte es más sencilla, simplemente debemos indicar que utiliza el servicio de DHCP, en el campo «dhcp» indicamos que queremos utilizar este servicio. La configuración quedaría así:

¿Cómo ver o cambiar el nombre actual del equipo?

Para ver el nombre de nuestro equipo lo podemos hacer utilizando dos comandos:
___
hostnamectl
___
O bien:
___
hostname
___
Para cambiar el nombre, podéis seguir las instrucciones de esta entrada, que escribí hace un tiempo: