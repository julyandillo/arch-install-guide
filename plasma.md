# Instalación de KDE Plasma con escritorio

Antes de instalar el entorno de escritorio necesitamos los paquetes del servidor gráfico y los controladores de nuestra tarjeta gráfica.

> \#sudo pacman -S xorg xorg-server

Dependiendo del modelo de gráfica que tengamos instalamos los drivers correspondientes:

#### Nvidia

> \# sudo pacman -S nvidia-dkms nvidia-utils nvidia-settings

A partir de Plasma 6 se usa Wayland por defecto (aunque es posible seguir usando Xorg si se elige en el inicio de sesión). Para poder usar Wayland con una gráfica Nvidia hay que realizar la siguiente configuración:

1. Modificar el archivo `/etc/default/grub` y añadir:

> \#GRUB_CMDLINE_LINUX_DEFAULT="<...> nvidia_drm.modeset=1"

2. Hablitar la siguiente configuración en el archivo `/etc/modprobe.d/nvidia-power-management.conf`:

> \#options nvidia NVreg_PreserveVideoMemoryAllocations=1

3. Habilitar los siguientes módulos en el archivo `/etc/mkinitcpio.conf`:

> \#MODULES="<...> nvidia nvidia_drm nvidia_uvm nvidia_modeset"

4. Generamos una nueva imagen de arranque:

> \#sudo mkinitcpio -P

5. Generamos un nuevo archivo de configuración para GRUB:

> \#sudo grub-mkconfig -o /boot/grub/grub.cfg

6. Por último habilitamos un par de servicios para no tener problemas al despetar de suspensión o de hibernación:

> \#sudo systemctl enable nvidia-suspend.service nvidia-hibernate.service


#### AMD

> \#sudo pacman -S xf86-video-amdgpu

#### Intel

> \#sudo pacman -S xf86-video-intel