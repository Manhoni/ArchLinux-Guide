# 📘 Guia Completo: Arch Linux com Hyprland
## Parte 2 de 3: Configuração Pós-Instalação e Hyprland

**Continuação da Parte 1**

---

## 📋 Conteúdo desta Parte:

1. Configuração Pós-Instalação
2. Conectar Wi-Fi (Completo)
3. Instalar yay (AUR Helper)
4. Configuração Completa do Hyprland
5. Configuração do Waybar
6. Programas Essenciais

---

## ⚙️ 1. Configuração Pós-Instalação

Você acabou de fazer login e o Hyprland está rodando! Agora vamos configurar tudo.

### **1.1 Abrir o Terminal:**

**Métodos para abrir terminal:**
- `SUPER + Q` (pode não funcionar na primeira vez)
- `SUPER + RETURN` (tecla Enter)
- `SUPER + D` → digite `foot` → ENTER
- `SUPER + D` → digite `kitty` → ENTER
- `SUPER + D` → digite `alacritty` → ENTER

**Se NENHUM funcionar:**
- Pressione `CTRL + ALT + F3` (TTY alternativo)
- Faça login
- Continue os comandos abaixo

---

### **1.2 Atualizar Sistema (Primeiro Comando!):**

```bash
# Atualizar base de dados e sistema
sudo pacman -Syu

# Vai pedir senha de sudo (a que você criou)
# Digite e pressione ENTER (não aparece enquanto digita)
```

**Se pedir confirmação:**
- Digite `Y` e ENTER
- Ou só ENTER (Y é padrão)

---

## 📡 2. Conectar Wi-Fi (Guia Completo)

### **2.1 Verificar Status da Conexão:**

```bash
# Ver todas as interfaces de rede
ip link

# Vai mostrar algo como:
# 1: lo: <LOOPBACK,UP,LOWER_UP>
# 2: enp2s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> (Ethernet)
# 3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> (Wi-Fi)

# Anotar nome da interface Wi-Fi (geralmente wlan0, wlp2s0, wlp3s0)
```

**Identificar sua interface Wi-Fi:**
- Procure por: `wlan0`, `wlp2s0`, `wlp3s0`, `wlo1`
- Anote o nome correto!

---

### **2.2 Garantir que NetworkManager está Rodando:**

```bash
# Verificar se NetworkManager está ativo
systemctl status NetworkManager

# Deve mostrar: "active (running)" em verde

# Se não estiver rodando:
sudo systemctl start NetworkManager
sudo systemctl enable NetworkManager
```

---

### **2.3 Método 1: nmcli (Mais Fácil):**

**O NetworkManager tem ferramenta de linha de comando!**

```bash
# Ver status Wi-Fi
nmcli radio wifi

# Se mostrar "disabled", habilitar:
nmcli radio wifi on

# Listar redes disponíveis
nmcli device wifi list

# Vai mostrar:
# IN-USE  SSID              MODE   CHAN  RATE        SIGNAL  BARS  SECURITY
#         MinhaRede         Infra  6     195 Mbit/s  75      ▂▄▆_  WPA2
#         VizinhoWiFi       Infra  11    130 Mbit/s  45      ▂▄__  WPA2

# Conectar à rede (substitua NOME_DA_REDE)
nmcli device wifi connect "NOME_DA_REDE" password "SUA_SENHA"

# Exemplo:
# nmcli device wifi connect "MinhaRede" password "senha12345"

# Se der erro de caractere especial na senha, use aspas simples:
nmcli device wifi connect "NOME_DA_REDE" password 'SUA_SENHA'
```

**Verificar conexão:**
```bash
# Ver status da conexão
nmcli connection show

# Testar internet
ping -c 3 google.com

# Se funcionar, sucesso! CTRL+C para parar
```

---

### **2.4 Método 2: nmtui (Interface Gráfica no Terminal):**

**Mais visual e fácil!**

```bash
# Abrir interface gráfica
nmtui

# Vai abrir um menu azul/colorido
# Use setas ↑↓ para navegar, ENTER para selecionar

# 1. Escolha "Activate a connection"
# 2. Navegue até sua rede Wi-Fi
# 3. Pressione ENTER
# 4. Digite a senha
# 5. ENTER
# 6. Volte com TAB até "Back" e ENTER
# 7. Quit

# Testar
ping -c 3 google.com
```

---

### **2.5 Método 3: iwctl (Método Manual):**

**Se NetworkManager não funcionar:**

```bash
# Iniciar iwctl
iwctl

# Dentro do iwctl (prompt muda para [iwd]#):

# 1. Listar dispositivos
device list
# Anote o nome (geralmente wlan0)

# 2. Escanear redes
station wlan0 scan
# (Não mostra nada, só escaneia)

# 3. Ver redes disponíveis
station wlan0 get-networks

# Vai mostrar:
# Network name            Security  Signal
# ────────────────────────────────────────
# MinhaRede               psk       ****
# VizinhoWiFi             psk       ***

# 4. Conectar (substitua NOME e wlan0 se necessário)
station wlan0 connect "NOME_DA_REDE"
# Vai pedir senha, digite e ENTER

# 5. Sair
exit

# Testar
ping -c 3 google.com
```

---

### **2.6 Reconectar Automaticamente:**

O NetworkManager salva a rede e reconecta automaticamente!

**Para ver redes salvas:**
```bash
nmcli connection show
```

**Para conectar a uma rede salva:**
```bash
nmcli connection up "NOME_DA_CONEXÃO"
```

**Para esquecer uma rede:**
```bash
nmcli connection delete "NOME_DA_CONEXÃO"
```

---

### **2.7 Problemas Comuns Wi-Fi:**

#### **Problema 1: "No wireless networks found"**

```bash
# Verificar se driver wireless está carregado
lspci -k | grep -A 3 -i network

# Deve mostrar algo como:
# Kernel driver in use: iwlwifi (Intel)
# ou: ath9k (Atheros)
# ou: rtw88 (Realtek)

# Se não mostrar driver, instalar firmware:
sudo pacman -S linux-firmware
reboot
```

#### **Problema 2: "Device not ready"**

```bash
# Verificar se interface não está bloqueada
rfkill list

# Se mostrar "Soft blocked: yes":
rfkill unblock wifi

# Ou específico:
rfkill unblock 0  # Número do dispositivo
```

#### **Problema 3: Wi-Fi desconecta constantemente**

```bash
# Desabilitar power saving
sudo iw dev wlan0 set power_save off

# Tornar permanente:
sudo nano /etc/NetworkManager/conf.d/wifi-powersave.conf

# Adicionar:
[connection]
wifi.powersave = 2

# Salvar (CTRL+O, ENTER, CTRL+X)
# Reiniciar NetworkManager:
sudo systemctl restart NetworkManager
```

---

## 📦 3. Instalar yay (AUR Helper)

O **AUR** (Arch User Repository) tem milhares de pacotes extras!
**yay** facilita instalar do AUR.

### **3.1 Instalação do yay:**

```bash
# Garantir que git e base-devel estão instalados
sudo pacman -S --needed git base-devel

# Ir para /tmp (temporário)
cd /tmp

# Clonar repositório do yay
git clone https://aur.archlinux.org/yay.git

# Entrar na pasta
cd yay

# Compilar e instalar
makepkg -si

# Vai perguntar confirmação, digite Y

# Voltar para home
cd ~

# Testar
yay --version
```

**Se der erro de PGP/GPG:**
```bash
# Atualizar chaves
sudo pacman-key --refresh-keys
sudo pacman -Syu archlinux-keyring
```

---

### **3.2 Usar o yay:**

```bash
# Procurar pacote
yay nome-do-pacote

# Instalar pacote
yay -S nome-do-pacote

# Atualizar tudo (Arch + AUR)
yay -Syu

# Remover pacote
yay -R nome-do-pacote

# Remover pacote + dependências não usadas
yay -Rns nome-do-pacote
```

---

### **3.3 Instalar Pacotes Úteis do AUR:**

```bash
# Interface para configurar temas GTK
yay -S nwg-look-bin

# Hyprpaper (wallpaper)
yay -S hyprpaper

# Spotify
yay -S spotify

# Visual Studio Code
yay -S visual-studio-code-bin

# Google Chrome
yay -S google-chrome

# Brave Browser
yay -S brave-bin
```

---

## 🎨 4. Configuração Completa do Hyprland

### **4.1 Estrutura de Diretórios:**

```bash
# Criar pastas de configuração
mkdir -p ~/.config/hypr
mkdir -p ~/.config/waybar
mkdir -p ~/.config/wofi
mkdir -p ~/.config/mako
mkdir -p ~/.config/kitty
mkdir -p ~/.config/foot
mkdir -p ~/Pictures/Wallpapers
mkdir -p ~/.config/hypr/scripts
```

---

### **4.2 Backup da Config Existente:**

```bash
# Se já existe config, fazer backup
cp ~/.config/hypr/hyprland.conf ~/.config/hypr/hyprland.conf.backup
```

---

### **4.3 Configuração Principal do Hyprland:**

```bash
# Editar arquivo principal
nano ~/.config/hypr/hyprland.conf
```

**Cole a configuração completa:**

```conf
# ════════════════════════════════════════════════════
# HYPRLAND CONFIGURATION
# Otimizado para Intel Graphics + Notebook
# ════════════════════════════════════════════════════

# ────────────────────────────────────────────────────
# MONITOR
# ────────────────────────────────────────────────────
monitor=,preferred,auto,1
# Ajuste conforme sua resolução:
# monitor=,1920x1080@60,auto,1    # Full HD
# monitor=,1366x768@60,auto,1     # HD comum em notebooks

# ────────────────────────────────────────────────────
# PROGRAMAS PADRÃO
# ────────────────────────────────────────────────────
$terminal = foot
# Alternativas: kitty, alacritty
$fileManager = thunar
$menu = wofi --show drun
$browser = firefox

# ────────────────────────────────────────────────────
# AUTOSTART
# ────────────────────────────────────────────────────
exec-once = waybar &
exec-once = mako &
exec-once = /usr/lib/polkit-gnome/polkit-gnome-authentication-agent-1 &
exec-once = swwbg ~/Pictures/Wallpapers/wallpaper.jpg &
# exec-once = nm-applet &
# exec-once = blueman-applet &

# ────────────────────────────────────────────────────
# VARIÁVEIS DE AMBIENTE
# ────────────────────────────────────────────────────
# Básicas
env = XCURSOR_SIZE,24
env = HYPRCURSOR_SIZE,24

# Wayland
env = XDG_CURRENT_DESKTOP,Hyprland
env = XDG_SESSION_TYPE,wayland
env = XDG_SESSION_DESKTOP,Hyprland

# Toolkits
env = GDK_BACKEND,wayland,x11
env = QT_QPA_PLATFORM,wayland;xcb
env = SDL_VIDEODRIVER,wayland
env = CLUTTER_BACKEND,wayland

# Intel Graphics
env = LIBVA_DRIVER_NAME,iHD
env = VDPAU_DRIVER,va_gl

# Teclado BR
env = XKB_DEFAULT_LAYOUT,br
env = XKB_DEFAULT_VARIANT,abnt2

# ────────────────────────────────────────────────────
# INPUT (Teclado, Mouse, Touchpad)
# ────────────────────────────────────────────────────
input {
    kb_layout = br
    kb_variant = abnt2
    kb_model = abnt2
    
    repeat_rate = 50
    repeat_delay = 300
    
    follow_mouse = 1
    sensitivity = 0
    
    touchpad {
        natural_scroll = true
        tap-to-click = true
        drag_lock = false
        disable_while_typing = true
    }
}

# ────────────────────────────────────────────────────
# GESTURES (Touchpad)
# ────────────────────────────────────────────────────
gestures {
    workspace_swipe = true
    workspace_swipe_fingers = 3
}

# ────────────────────────────────────────────────────
# GENERAL
# ────────────────────────────────────────────────────
general {
    gaps_in = 5
    gaps_out = 10
    border_size = 2
    
    col.active_border = rgba(33ccffee) rgba(00ff99ee) 45deg
    col.inactive_border = rgba(595959aa)
    
    layout = dwindle
}

# ────────────────────────────────────────────────────
# DECORATION
# ────────────────────────────────────────────────────
decoration {
    rounding = 10
    
    active_opacity = 1.0
    inactive_opacity = 0.95
    
    blur {
        enabled = true
        size = 3
        passes = 1
    }
    
    drop_shadow = yes
    shadow_range = 4
    shadow_render_power = 3
    col.shadow = rgba(1a1a1aee)
}

# ────────────────────────────────────────────────────
# ANIMATIONS
# ────────────────────────────────────────────────────
animations {
    enabled = yes
    
    bezier = myBezier, 0.05, 0.9, 0.1, 1.05
    
    animation = windows, 1, 7, myBezier
    animation = windowsOut, 1, 7, default, popin 80%
    animation = border, 1, 10, default
    animation = fade, 1, 7, default
    animation = workspaces, 1, 6, default
}

# ────────────────────────────────────────────────────
# LAYOUTS
# ────────────────────────────────────────────────────
dwindle {
    pseudotile = yes
    preserve_split = yes
}

# ────────────────────────────────────────────────────
# MISC
# ────────────────────────────────────────────────────
misc {
    disable_hyprland_logo = true
    disable_splash_rendering = true
    vfr = true
    mouse_move_enables_dpms = false
    key_press_enables_dpms = false
}

# ════════════════════════════════════════════════════
# KEYBINDINGS
# ════════════════════════════════════════════════════
$mainMod = SUPER

# Aplicativos
bind = $mainMod, RETURN, exec, $terminal
bind = $mainMod, Q, killactive
bind = $mainMod, M, exit
bind = $mainMod, E, exec, $fileManager
bind = $mainMod, B, exec, $browser
bind = $mainMod, V, togglefloating
bind = $mainMod, D, exec, $menu
bind = $mainMod, F, fullscreen

# Screenshots
bind = , PRINT, exec, grim -g "$(slurp)" - | wl-copy
bind = $mainMod, PRINT, exec, grim ~/Pictures/screenshot-$(date +%Y%m%d-%H%M%S).png

# Navegação (Setas)
bind = $mainMod, left, movefocus, l
bind = $mainMod, right, movefocus, r
bind = $mainMod, up, movefocus, u
bind = $mainMod, down, movefocus, d

# Navegação (Vim keys)
bind = $mainMod, H, movefocus, l
bind = $mainMod, L, movefocus, r
bind = $mainMod, K, movefocus, u
bind = $mainMod, J, movefocus, d

# Mover janelas
bind = $mainMod SHIFT, left, movewindow, l
bind = $mainMod SHIFT, right, movewindow, r
bind = $mainMod SHIFT, up, movewindow, u
bind = $mainMod SHIFT, down, movewindow, down

# Workspaces
bind = $mainMod, 1, workspace, 1
bind = $mainMod, 2, workspace, 2
bind = $mainMod, 3, workspace, 3
bind = $mainMod, 4, workspace, 4
bind = $mainMod, 5, workspace, 5
bind = $mainMod, 6, workspace, 6
bind = $mainMod, 7, workspace, 7
bind = $mainMod, 8, workspace, 8
bind = $mainMod, 9, workspace, 9
bind = $mainMod, 0, workspace, 10

# Mover para workspaces
bind = $mainMod SHIFT, 1, movetoworkspace, 1
bind = $mainMod SHIFT, 2, movetoworkspace, 2
bind = $mainMod SHIFT, 3, movetoworkspace, 3
bind = $mainMod SHIFT, 4, movetoworkspace, 4
bind = $mainMod SHIFT, 5, movetoworkspace, 5
bind = $mainMod SHIFT, 6, movetoworkspace, 6
bind = $mainMod SHIFT, 7, movetoworkspace, 7
bind = $mainMod SHIFT, 8, movetoworkspace, 8
bind = $mainMod SHIFT, 9, movetoworkspace, 9
bind = $mainMod SHIFT, 0, movetoworkspace, 10

# Mouse
bindm = $mainMod, mouse:272, movewindow
bindm = $mainMod, mouse:273, resizewindow

# Teclas Multimídia (Notebook)
bind = , XF86AudioRaiseVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%+
bind = , XF86AudioLowerVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-
bind = , XF86AudioMute, exec, wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle
bind = , XF86AudioPlay, exec, playerctl play-pause
bind = , XF86AudioNext, exec, playerctl next
bind = , XF86AudioPrev, exec, playerctl previous
bind = , XF86MonBrightnessUp, exec, brightnessctl set +5%
bind = , XF86MonBrightnessDown, exec, brightnessctl set 5%-

# ────────────────────────────────────────────────────
# WINDOW RULES
# ────────────────────────────────────────────────────
windowrule = float, ^(pavucontrol)$
windowrule = float, ^(blueman-manager)$
windowrulev2 = opacity 0.95 0.95, class:^(thunar)$
```

**Salvar:**
- `CTRL + O` → ENTER → `CTRL + X`

---

### **4.4 Recarregar Configuração:**

```bash
# Recarregar sem sair
hyprctl reload

# Ou sair e entrar novamente
# SUPER + M (sair)
# Fazer login de novo
```

---

## 🎯 5. Configuração do Waybar

### **5.1 Arquivo de Configuração:**

```bash
nano ~/.config/waybar/config
```

**Cole:**

```json
{
    "layer": "top",
    "position": "top",
    "height": 32,
    
    "modules-left": ["hyprland/workspaces", "hyprland/window"],
    "modules-center": ["clock"],
    "modules-right": ["pulseaudio", "network", "cpu", "memory", "battery", "tray"],
    
    "hyprland/workspaces": {
        "format": "{icon}",
        "on-click": "activate",
        "format-icons": {
            "1": "1",
            "2": "2",
            "3": "3",
            "4": "4",
            "5": "5",
            "active": "",
            "default": ""
        }
    },
    
    "clock": {
        "format": "{:%H:%M}",
        "format-alt": "{:%A, %d de %B - %H:%M}",
        "tooltip-format": "<small>{calendar}</small>"
    },
    
    "cpu": {
        "format": " {usage}%"
    },
    
    "memory": {
        "format": " {}%"
    },
    
    "battery": {
        "states": {
            "warning": 30,
            "critical": 15
        },
        "format": "{icon} {capacity}%",
        "format-charging": " {capacity}%",
        "format-icons": ["", "", "", "", ""]
    },
    
    "network": {
        "format-wifi": " {essid}",
        "format-ethernet": " {ipaddr}",
        "format-disconnected": "⚠ Desconectado",
        "on-click": "nm-connection-editor"
    },
    
    "pulseaudio": {
        "format": "{icon} {volume}%",
        "format-muted": " Mudo",
        "format-icons": ["", "", ""],
        "on-click": "pavucontrol"
    },
    
    "tray": {
        "spacing": 10
    }
}
```

**Salvar:** `CTRL + O` → ENTER → `CTRL + X`

---

### **5.2 Estilo do Waybar:**

```bash
nano ~/.config/waybar/style.css
```

**Cole:**

```css
* {
    font-family: "JetBrainsMono Nerd Font";
    font-size: 13px;
}

window#waybar {
    background: rgba(30, 30, 46, 0.9);
    color: #cdd6f4;
}

#workspaces button {
    padding: 0 10px;
    color: #cdd6f4;
}

#workspaces button.active {
    background: rgba(137, 180, 250, 0.3);
    border-bottom: 2px solid #89b4fa;
}

#clock, #battery, #cpu, #memory, #network, #pulseaudio {
    padding: 0 10px;
    margin: 0 2px;
}

#battery.charging { color: #a6e3a1; }
#battery.warning { color: #f9e2af; }
#battery.critical { color: #f38ba8; }
```

**Salvar:** `CTRL + O` → ENTER → `CTRL + X`

---

### **5.3 Reiniciar Waybar:**

```bash
# Matar processo atual
killall waybar

# Iniciar novo
waybar &
```

---

## 📦 6. Programas Essenciais

### **6.1 Instalar Pacotes Úteis:**

```bash
# Ferramentas de sistema
sudo pacman -S \
    man-db man-pages \
    wget curl \
    unzip p7zip unrar \
    playerctl

# Fontes
sudo pacman -S \
    ttf-dejavu \
    ttf-liberation \
    noto-fonts \
    noto-fonts-emoji \
    ttf-jetbrains-mono-nerd

# Multimídia
sudo pacman -S \
    mpv \
    imv \
    zathura zathura-pdf-mupdf

# Utilitários Wayland
sudo pacman -S \
    wl-clipboard \
    cliphist \
    swww

# Bluetooth
sudo pacman -S \
    bluez \
    bluez-utils \
    blueman

# Gerenciamento de energia
sudo pacman -S \
    tlp \
    powertop

# Habilitar TLP (economia de bateria)
sudo systemctl enable --now tlp
```

---

### **6.2 Programas do Dia a Dia:**

```bash
# LibreOffice
sudo pacman -S libreoffice-fresh

# GIMP (edição de imagem)
sudo pacman -S gimp

# VLC
sudo pacman -S vlc

# Telegram
sudo pacman -S telegram-desktop

# Discord
sudo pacman -S discord

# Spotify (via yay)
yay -S spotify

# VS Code
yay -S visual-studio-code-bin
```

---

## ✅ Checklist Final da Parte 2:

- ✅ Wi-Fi configurado e funcionando
- ✅ yay instalado
- ✅ Hyprland configurado
- ✅ Waybar configurado e funcionando
- ✅ Programas essenciais instalados
- ✅ Teclado BR-ABNT2 funcionando

---

## ➡️ Próximos Passos:

**Continue na Parte 3 para:**
- Problemas comuns e soluções
- Dual Boot com Windows
- Comandos importantes do Hyprland
- Scripts de backup e restauração
- Otimizações finais

---

**Fim da Parte 2 de 3**

*Quando quiser a Parte 3, é só pedir!*