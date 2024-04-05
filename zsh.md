Para instalar zsh:

> \# sudo pacman -S zsh zsh-completions

Para instalar oh-my-zsh:

> \# sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

Instalar zsh-syntax-highlighting

> \# git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

Instalar zsh-autosuggestions

> \# git clone https://github.com/zsh-users/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

Después de instalar los plugins es necesario editar el archivo .zshrc para añadir los plugins en la línea que comienza con plugins=(  ..  nombre de los plugins separados por espacios ..)

Antes de instalar powerlevel 10k tenemos que instalar algunas fuentes necesarias para que funcione correctamente:

> \# yay -S ttf-dejavu ttf-meslo-nerd-font-powerlevel10k

Para instalar powerlevel10k:

> \# git clone --depth=1 https://github.com/romkatv/powerlevel10k.git \${ZSH_CUSTOM:-\$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

Para activar el tema es necesario editar el archivo .zshrc y colocar 'powerlevel10k/powerlevel10k' como el tema en uso.  
La primera vez que se ejecuta aparecerá automáticamente el asistente para configurar el tema, pero siempre se puede volver a lanzar el asistente con el siguiente comando:

> \# p10k configure
