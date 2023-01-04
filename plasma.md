# Intalacion de KDE Plasma con escritorio

Antes de instalar el entorno de escritorio necesitamos los paquetes del servidor gráfico y los controladores de nuestra tarjeta gráfica.

> \#sudo pacman -S xorg xorg-server

Dependiendo del modelo de gráfica que tengamos instalamos los drivers correspondientes:

#### Nvidia

> \# sudo pacman -S nvidia nvidia-utils nvidia-settings

#### AMD

> \#sudo pacman -S xf86-video-amdgpu

#### Intel

> \#sudo pacman -S xf86-video-intel