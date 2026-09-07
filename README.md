# OpenBox

O Openbox é um window manager (gerenciador de janelas) leve para Linux. Ele é muito usado com Ubuntu e outras distribuições quando você quer um desktop minimalista, rápido e altamente configurável.

Alguns pontos importantes:

* Gerencia as janelas, mas não é um desktop environment completo como GNOME ou KDE.
* É extremamente leve e consome poucos recursos.
* Permite configurar atalhos de teclado para praticamente qualquer ação.
* Pode ter menus acionados com o botão direito do mouse.
* É possível personalizar temas, bordas, cores e comportamento das janelas.
* A configuração tradicional é feita principalmente através de arquivos XML.
* Pode ser combinado com ferramentas como Tint2, Rofi, Polybar, feh, Picom etc. para montar um desktop completo.

## Install

    sudo apt-get install openbox
    
    openbox --version

## Tools

### Tint2

Tint2 — uma barra de tarefas/painel leve. Mostra janelas abertas, área de trabalho, bandeja do sistema, relógio etc. É uma ótima combinação com Openbox.

    sudo apt install tint2
    
    tint2 --version

Executar

    tint2 &

### Rofi

Rofi — um lançador de aplicativos e menu extremamente configurável. Você pode apertar uma tecla, digitar firefox e abrir o programa. Também pode funcionar como seletor de janelas, comandos e outros menus.

    sudo apt install rofi

    rofi -v

Buscar aplicativo para executar

    rofi -show drun

### Polybar

Polybar — outra barra/painel, mais moderna e altamente configurável. É muito popular em setups minimalistas e tiling window managers. Pode mostrar CPU, RAM, rede, bateria, música, workspaces etc.

    sudo apt install polybar
    
    polybar --version

### Feh

feh — um visualizador de imagens simples e leve. Em setups com Openbox, é frequentemente usado para definir o wallpaper:

    sudo apt install feh
    
    feh --version

    feh image.jpg

Alterar o papel de parede

    feh --bg-fill ~/Imagens/wallpaper.jpg

### Picom

Picom — um compositor para X11. Adiciona efeitos visuais como transparência, sombras, fade e algumas animações, além de ajudar a evitar certos problemas de renderização.

    sudo apt install picom
    
    picom --version

Executar

    picom &

## Structure

Como eles se encaixam

Um desktop Openbox poderia ficar assim:

┌─────────────────────────────────────────────┐
│              Polybar / Tint2                │
├─────────────────────────────────────────────┤
│                                             │
│                  Openbox                    │
│                                             │
│          suas janelas e aplicativos         │
│                                             │
└─────────────────────────────────────────────┘
          ↑                                        ↑
        Rofi                 Picom
    (lançador)           (efeitos visuais)

             feh → wallpaper

Openbox = gerencia as janelas
Tint2/Polybar = barra
Rofi = lançador/menu
feh = wallpaper/imagens
Picom = efeitos visuais/composição

## Uninstall

O processo de desinstalação teve alguns passos a mais do que o esperado. Durante a troca do gerenciador de janelas entre GNOME, XFCE e OpenBox. A variável "display-manager" perdeu o symbolic link para o GDM3.

O Sistema fez o reboot em TTY3 pois estava sem configurações padrão.
Verificar se nada referente ao GNOME display foi desinstalado.
    
    sudo apt-get install ubuntu-desktop gnome-shell gdm3    

    sudo systemctl start gdm3

O Sistema mudou automaticamente para o TTY7, ali precisei atribuir o valor referente ao GDM3 manualmente para voltar ao normal:

    sudo systemctl enable gdm3
    sudo systemctl get-default
    sudo systemctl set-default graphical.target
    sudo systemctl enable display-manager
    sudo rm -f /etc/systemd/system/display-manager.service
    sudo ln -s /lib/systemd/system/gdm3.service /etc/systemd/system/display-manager.service
    sudo systemctl daemon-reload
    sudo systemctl set-default graphical.target
    sudo ls -l /etc/systemd/system/display-manager.service
    sudo systemctl start display-manager

