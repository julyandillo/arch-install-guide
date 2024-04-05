## Instalación de Cinnamon como escritorio 

Instalamos cinnamon y algunas aplicaciones básicas:

> \# sudo pacman -S cinnamon gnome-terminal nemo-fileroller 

Instalar lightdm como display manager: 

> \# sudo pacman -S lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings

Después lo habilitamos para que se ejecute al inicio del sistema: 

> \# sudo systemctl enable lightdm.service

