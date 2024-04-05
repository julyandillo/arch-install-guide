# Instalación del sistema base

Toda la configuración de lenguajes que aquí se describe se realiza para Español de España.
Para obtener la configuración adecuada para otros lenguajes se puede consultar la guía de instalación de Arch
en la [wiki oficial](https://wiki.archlinux.org/title/Installation_guide).

## Definición de la distribución del teclado

> \# loadkeys es

## Verificar la modalidad de arranque

Si al ejecutar un ls sobre la siguiente ruta se lista el contenido de la carpeta sin errores, el sistema se iniciará en modo UEFI

> \# ls /sys/firware/efi/efivars

## Configurar el reloj

> \# timedatectl set-ntp true

Si vamos a tener Windows en la misma máquina para corregir el problema de la hora hay que ejecutar lo siguiente:

> \# timedatectl set-local-rtc 1

## Crear y montar particiones

Para crear las particiones usamos

> \# cfdisk /dev/sdx

donde *sdx* habrá que sustituirlo por el dispositivo donde queramos crear las particiones.

En una instalación básica se recomienda tener como mínimo 2 particiones, 512MB para la partición de arranque y otra del tamaño deseado para la instalación del sistema. También podemos crear una partición adicional para la swap del sistema, pero en mi caso prefiero configurar la swap como un fichero (para conocer más detalles sobre la gestión de la swap en arch visitar la [wiki](https://wiki.archlinux.org/title/Swap))

Una vez creadas las particiones, hay que darles el formato correcto y montarlas dentro de */mnt*. En este caso, vamos a suponer que hemos hecho 3 particiones, una para el arranque, otra para el sistema base y la home, y una independiente para almecenar datos.

> \# mkfs.fat -F 32 /dev/sda1 (partición para el arranque del sistema en FAT32)
> \# mkfs.ext4 /dev/sda2 (está será la partición raíz del sistema)
> \# mkfs.ext4 /dev/sda3 (partición adicional para almecenar datos)

Antes de montar las particiones hay que crear las carpetas de destino del punto de montaje

> \# mount /dev/sda2 /mnt
> \# mkdir -p /mnt/boot/efi  
> \# mount /dev/sda1 /mnt/boot/efi
> \# mkdir /mnt/datos  
> \# mount /dev/sda3 /mnt/datos

### Creación de un fichero como swap

Creamos un fichero con un tamaño de por ejemplo 4Gb

> \# mkswap -U clear --size 4G --file /swapfile

Activamos el fichero como swap

> \# swapon /swapfile

Añadimos al fichero `/etc/fstab` una nueva línea para que se monte automáticamente al iniciar el sistema

>/swapfile  none    swap    defaults    0   0


## Instalación de los paquetes básicos del sistema

> \# pacstrap /mnt base base-devel linux linux-firmware nano networkmanager dhcp [intel-ucode|amd-ucode] man-pages man-pages-es texinfo ntp

Después de instalar los paquetes básicos del sistema generamos el archivo fstab

> \# genfstab -U / mnt >> /mnt/etc/fstab

(con -U o -L especificamos, respectivamente, si queremos que en el fstab aparezcan las UUID o las etiquetas de las particiones)

Una vez hecho esto, ya podemos cambiar al nuevo sistema

> \# arch-chroot /mnt

## Configuración del sistema

## Zona horaria

> \# ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime
> \# hwclock --systohc

Para configurar el reloj automáticamente con un servidor ntp

> \# ntpdate -u hora.roa.es

### Idioma del sistema

Editar el archivo `/etc/locale.gen` y descomentar las líneas para el español de España `es_ES.UTF-8 UTF-8` y `en_US.UTF-8 UTF-8`. Después generamos los locales ejecutando:

> \# locale-gen

Definimos el español como idioma del sistema:

> \# echo "LANG=es_ES.UTF-8" > /etc/locale.conf

Y español como la distribución del teclado para que permanezca después de cada reinicio:

> \# echo "KEYMAP=es" > /etc/vconsole.conf

### Configurar la red

> \# echo "elnombredemiequipo" > /etc/hostname

Añadimos las siguientes líneas al fichero `/etc/hosts`
> 127.0.0.1     localhost  
> ::1           localhost  
> 127.0.1.1     elnombredemiequipo.localdomain elnombredemiequipo

### Contraseña de root

> \# passwd

### Agregar un usuario

Agregamos un nuevo usuario y lo metemos dentro del grupo `wheel`:

> \# useradd -mG wheel julyandillo  
> \# passwd julyandillo

Ahora modificamos el archivo sudoers para permitir al nuevo usuario hacer `sudo`

> \# EDITOR=nano visudo

Hay que descomentar la línea al final de fichero que comienza por `%wheel ...`

## Instalar un gestor de arranque

Instalaremos GRUB como gestor de arranque de nuestro sistema, además de utilidades para gestionar particiones ntfs.
Si vamos a compartir instalación con Windows, además habrá que instalar el paquete `os-prober`

> \# pacman -S grub efibootmgr mtools ntfs-3g dosfstools os-prober
> \# grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
> \# grub-mkconfig -o /boot/grub/grub.cfg

Si queremos añadir Windows como opción de arranque, hay que asegurarse que antes de ejecutar el anterior comando
tenemos montada la partición arranque de Windows (EFI Windows). Por ejemplo, podemos montarla dentro de /mnt/efi, generar el archivo de configuración de GRUB y después desmontarla y borrar la carpeta.

## Habilitar NetworkManager

Para que después de reiniciar el sistema de conecte a la red, debemos habilitarlo con para que se ejecute al inicio:

> \# systemctl enable NetworkManger

## Desmontamos particiones y reiniciamos el sistema

> \# exit (para salir del sistema, sino no dejará desmontar las particiones)
> \# umount -R /mnt
> \# reboot
