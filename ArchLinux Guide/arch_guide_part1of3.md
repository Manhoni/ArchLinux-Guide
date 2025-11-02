# 📘 Guia Completo: Arch Linux com Hyprland
## Parte 1 de 3: Preparação e Instalação

**Baseado em experiência real de instalação com troubleshooting completo**

---

## 📑 Índice Geral do Guia

### **Parte 1 (Este arquivo):**
1. Preparação e Hardware
2. Criação do Pendrive Bootável
3. Instalação Passo a Passo do Arch
4. Configurações do archinstall

### **Parte 2:**
5. Configuração Pós-Instalação
6. Configuração Completa do Hyprland
7. Configuração do Waybar
8. Programas Essenciais

### **Parte 3:**
9. Problemas Comuns e Soluções
10. Dual Boot com Windows
11. Comandos Importantes do Hyprland
12. Scripts de Backup e Restauração

---

## 🎯 1. Preparação e Hardware

### **1.1 Hardware Testado (Seu Notebook):**
- **CPU:** 2 núcleos / 4 threads ✅
- **RAM:** 12 GB ✅
- **GPU:** Intel integrada ✅
- **Disco:** SSD recomendado (30-50 GB mínimo)

**✅ Seu hardware é PERFEITO para Hyprland!**

### **1.2 Requisitos Mínimos:**
- **CPU:** 2+ cores (qualquer processador moderno)
- **RAM:** 4 GB mínimo, 8+ GB ideal
- **GPU:** Qualquer (Intel, AMD, NVIDIA)
- **Disco:** 20 GB mínimo, 50 GB recomendado
- **Internet:** Necessária durante instalação

### **1.3 Sobre o Arch Linux:**

**O que você precisa saber:**
- ✅ **Rolling Release:** Sempre atualizado, sem versões
- ✅ **Não existe Arch LTS:** Sempre na versão mais recente
- ✅ **Comunidade:** Wiki excelente, fóruns ativos
- ⚠️ **Curva de aprendizado:** Alta, mas você aprende DE VERDADE
- ⚠️ **Manutenção:** Requer atualizações frequentes

**Por que Arch é "difícil":**
- Nada funciona automaticamente
- Você precisa configurar tudo
- Precisa saber o que está instalando
- MAS: Você aprende como Linux realmente funciona!

### **1.4 Sobre o Hyprland:**

**O que é:**
- Compositor Wayland moderno
- Window Manager tiling (janelas lado a lado)
- Animações suaves, configurável
- Leve e rápido

**Requer:**
- Aceleração gráfica (GPU)
- Wayland (não funciona com X11 puro)
- Aprendizado de atalhos de teclado

---

## 📥 2. Download e Criação do Pendrive

### **2.1 Baixar a ISO do Arch:**

**Site Oficial:**
- https://archlinux.org/download/
- Escolha um mirror brasileiro para velocidade
- Versão testada: **2025.10.01** (Kernel 6.16.8)
- Tamanho: ~1.4 GB

**Verificar integridade (opcional mas recomendado):**
```bash
# No Linux
sha256sum archlinux-2025.10.01-x86_64.iso
# Compare com o valor no site
```

### **2.2 Criar Pendrive Bootável:**

#### **No Linux (dd):**
```bash
# 1. Identificar o pendrive
lsblk
# Procure por algo como /dev/sdb (8GB+ sem partições montadas)

# 2. Desmontar se estiver montado
sudo umount /dev/sdb*

# 3. Gravar ISO (CUIDADO! Apaga tudo no pendrive)
sudo dd if=archlinux-2025.10.01-x86_64.iso of=/dev/sdb bs=4M status=progress oflag=sync

# 4. Sincronizar
sync

# ATENÇÃO: Substitua /dev/sdb pelo SEU pendrive!
# Usar o device errado vai APAGAR dados!
```

#### **No Windows (Rufus):**

1. **Baixar Rufus:** https://rufus.ie/
2. **Configurações:**
   - **Dispositivo:** Seu pendrive
   - **Seleção de boot:** Escolha a ISO do Arch
   - **Esquema de partição:** **GPT** ← IMPORTANTE!
   - **Sistema de destino:** **UEFI (não CSM)** ← IMPORTANTE!
   - **Sistema de arquivos:** FAT32
   - **Tamanho do cluster:** Padrão (4096)
3. **Iniciar** → Confirmar → Aguardar
4. **Fechar** quando terminar

#### **No macOS (dd):**
```bash
# Similar ao Linux
diskutil list
# Identificar o pendrive (disk2, disk3, etc)

diskutil unmountDisk /dev/diskN
sudo dd if=archlinux-2025.10.01-x86_64.iso of=/dev/rdiskN bs=1m
# Use /dev/rdisk para velocidade maior
```

---

## 🔧 3. Preparação da BIOS/UEFI

### **3.1 Desabilitar Secure Boot (CRÍTICO!):**

**Por que?**
- Secure Boot bloqueia sistemas não assinados pela Microsoft
- Arch Linux não é assinado pela Microsoft
- Você DEVE desabilitar para o Arch iniciar

**Como:**

```
1. Reinicie o computador
2. Durante o boot, pressione repetidamente:
   - Del (mais comum)
   - F2 (comum em notebooks)
   - F10 (HP)
   - F12 (Lenovo, Dell)
   - Esc (alguns modelos)
   
3. Na BIOS/UEFI:
   - Procure seção "Boot" ou "Security"
   - Encontre "Secure Boot"
   - Mude para "Disabled"
   
4. Salvar e Sair:
   - Geralmente F10
   - Ou opção "Save & Exit"
```

**⚠️ MUITO IMPORTANTE:**
- **NÃO desabilite UEFI!**
- Apenas Secure Boot
- UEFI deve permanecer **ATIVADO**
- Você não precisará fazer isso toda vez!

### **3.2 Verificar Modo de Boot:**

Certifique-se que está em:
- ✅ **UEFI Mode** (não Legacy/CSM)
- ✅ **Fast Boot:** Pode deixar ativado (mas desabilitar ajuda)

### **3.3 Ordem de Boot:**

Na BIOS, configure:
```
1º - USB/Removable Device
2º - Hard Disk/SSD
```

Ou use o menu de boot temporário (F12/F11) na hora.

---

## 🚀 4. Instalação do Arch Linux

### **4.1 Boot pelo Pendrive:**

1. **Inserir pendrive** no notebook
2. **Reiniciar**
3. **Pressionar F12/F11/F9** (depende do notebook) para menu de boot
   - Ou deixar bootar automático se USB for primeiro na ordem
4. **Selecionar** o pendrive USB
5. **Escolher** "Arch Linux install medium" (primeira opção)
6. **Aguardar** carregar (alguns segundos)

**Você vai ver:**
```
Arch Linux 6.16.8-arch1 (tty1)

archiso login: root (automatic login)

root@archiso ~ #
```

Pronto! Você está no Arch Linux live!

---

### **4.2 Configurar Teclado (Opcional):**

Por padrão, o teclado está em US (inglês).

**Para mudar para BR-ABNT2:**
```bash
loadkeys br-abnt2
```

**Testar:**
- Digite `ç`, `~`, `´` para confirmar
- Se funcionar, sucesso!

---

### **4.3 Conectar à Internet:**

#### **Ethernet (Cabo):**
```bash
# Já funciona automaticamente!
# Apenas teste:
ping -c 3 google.com

# Se funcionar, pule para próximo passo
```

#### **Wi-Fi:**
```bash
# 1. Iniciar iwctl
iwctl

# 2. Dentro do iwctl, listar redes
station wlan0 scan
station wlan0 get-networks

# 3. Conectar (substitua NOME_DA_REDE)
station wlan0 connect "NOME_DA_REDE"
# Digite a senha quando pedir

# 4. Sair
exit

# 5. Testar
ping -c 3 google.com
```

**Se der erro "No such device":**
```bash
# Ver interfaces disponíveis
ip link

# Pode ser wlan1, wlp2s0, etc
# Use o nome correto no lugar de wlan0
```

**Testar conexão:**
```bash
ping -c 3 google.com

# Deve ver:
# 64 bytes from ... time=Xms
# Pressione CTRL+C para parar
```

---

### **4.4 Sincronizar Relógio:**

```bash
timedatectl set-ntp true
```

**Verificar:**
```bash
timedatectl status
# Deve mostrar "System clock synchronized: yes"
```

---

### **4.5 Iniciar o Instalador Interativo:**

O Arch agora tem um instalador oficial chamado **archinstall**!

```bash
archinstall
```

**Você verá um menu colorido!**

---

## ⚙️ 5. Configurações do archinstall

Navegue com **setas ↑↓** e **ENTER** para selecionar.

### **5.1 Archinstall Language:**
- Escolha: `Portuguese (Brazil)` 🇧🇷
- Ou deixe: `English` (mais documentação disponível)

---

### **5.2 Mirrors (Espelhos de Download):**
- **Opção 1:** Deixe automático (ele escolhe os mais rápidos)
- **Opção 2:** Selecione `Brazil` manualmente

**Por que importa?**
- Mirrors no Brasil = download mais rápido
- Mas automático funciona bem!

---

### **5.3 Locales (Idioma e Região):**

#### **Keyboard layout:**
- Escolha: `br-abnt2` ✅
- Ou: `br-abnt` (similar)

#### **Locale language:**
- `pt_BR.UTF-8` (sistema em português)
- Ou `en_US.UTF-8` (recomendado - mais documentação)

**Dica:** Use `en_US.UTF-8` no sistema, BR no teclado!

#### **Locale encoding:**
- Deixe: `UTF-8` ✅

---

### **5.4 Timezone:**
- Escolha: `America/Sao_Paulo` ✅
- Ou sua cidade (se disponível)

---

### **5.5 Disk Configuration (MUITO IMPORTANTE!):**

**⚠️ LEIA COM ATENÇÃO!**

#### **Para instalação ÚNICA (só Arch, sem Windows):**

1. Selecione: **"Use a best-effort default partition layout"** ✅
2. Escolha o disco:
   - `/dev/sda` (discos SATA/padrão)
   - `/dev/nvme0n1` (discos NVMe/M.2)
3. **Filesystem:** `ext4` ✅ **RECOMENDADO!**
   - Mais simples e estável
   - Btrfs pode dar problemas
4. **Confirme:** YES (vai formatar o disco!)

**⚠️ ISSO VAI APAGAR TUDO NO DISCO SELECIONADO!**

#### **Para Dual Boot (Windows + Arch):**
- **NÃO use esta opção!**
- Veja seção de Dual Boot na Parte 3
- Precisa particionar manualmente

---

### **5.6 Disk Encryption (Opcional):**
- **Encryption password:** Deixe vazio (sem criptografia)
- Ou configure senha se quiser disco criptografado

**Recomendação:** Deixe sem criptografia na primeira instalação.

---

### **5.7 Bootloader:**
- Escolha: **Grub** ✅
- É o mais compatível
- Detecta Windows automaticamente (dual boot)

---

### **5.8 Swap (Memória Virtual):**
- **True** (sim) - recomendado
- Ou **False** se você tem muita RAM (16GB+)

**Com 12GB RAM:** Deixe True (recomendado)

---

### **5.9 Hostname:**
- Nome do seu computador
- Exemplo: `archlinux`, `meunotebook`, `arch-pc`
- Use apenas letras minúsculas, números e hífen

---

### **5.10 Root Password:**

**⚠️ IMPORTANTE:**
- Você pode deixar vazio (não ter senha de root)
- Ou definir uma senha forte
- **Recomendado:** Deixe vazio, use apenas sudo

---

### **5.11 User Account (CRÍTICO!):**

1. **Add a user:** Sim! ✅
2. **Username:** Seu nome de login (letras minúsculas)
3. **Password:** Senha forte! Você vai usar MUITO
4. **⚠️ IMPORTANTE:** Marque como **superuser (sudo)** ✅

**Dica para senha:**
- Use algo fácil de digitar (você vai usar muito!)
- Mas forte (mínimo 8 caracteres, letras+números)
- Anote se necessário!

---

### **5.12 Profile (Desktop Environment):**

1. Selecione: **Desktop** ✅
2. Escolha: **Hyprland** ✅
3. Vai instalar Hyprland + dependências básicas

---

### **5.13 Audio:**
- Escolha: **Pipewire** ✅
- É mais moderno que PulseAudio
- Funciona melhor com Wayland

---

### **5.14 Network Configuration:**
- Escolha: **NetworkManager** ✅
- Mais fácil de usar
- GUI disponível
- Suporta Wi-Fi, VPN, etc

---

### **5.15 Additional Packages (MUITO IMPORTANTE!):**

**⚠️ COPIE E COLE TUDO DE UMA VEZ!**

**Para seu notebook Intel:**
```
kitty alacritty foot waybar wofi rofi firefox thunar git base-devel nano vim htop btop neofetch networkmanager intel-ucode mesa vulkan-intel libva-intel-driver grim slurp wl-clipboard pavucontrol brightnessctl bluez bluez-utils
```

**Explicação dos pacotes:**

| Pacote | O que faz |
|--------|-----------|
| `kitty alacritty foot` | Terminais (foot melhor em VMs) |
| `waybar` | Barra de status no topo |
| `wofi rofi` | Lançadores de apps (SUPER+D) |
| `firefox` | Navegador |
| `thunar` | Gerenciador de arquivos |
| `git base-devel` | Ferramentas dev (para AUR) |
| `nano vim` | Editores de texto |
| `htop btop` | Monitores de sistema |
| `neofetch` | Info do sistema (bonito!) |
| `networkmanager` | Gerenciar rede |
| `intel-ucode` | **CRÍTICO para Intel!** Microcódigo |
| `mesa vulkan-intel` | Drivers gráficos Intel |
| `libva-intel-driver` | Aceleração de vídeo Intel |
| `grim slurp` | Screenshots no Wayland |
| `wl-clipboard` | Clipboard Wayland |
| `pavucontrol` | Controle de áudio (GUI) |
| `brightnessctl` | Controle de brilho (notebook) |
| `bluez bluez-utils` | Bluetooth |

---

### **5.16 Optional Repositories:**
- Deixe padrão (nada selecionado)
- Ou marque **multilib** se for usar Wine/Steam (32-bit)

---

### **5.17 Polkit (pode aparecer):**

Se perguntar sobre polkit:
- Escolha: **polkit** ✅
- **NÃO escolha:** seatd

---

### **5.18 Revisão Final:**

1. **Revise TUDO!**
2. Principalmente:
   - ✅ Usuário criado e com sudo
   - ✅ Disco correto selecionado
   - ✅ Hyprland selecionado
   - ✅ Pacotes adicionais incluídos
3. Selecione: **Install** ✅
4. **Confirme:** YES

---

## ⏳ 6. Processo de Instalação

### **6.1 O que vai acontecer:**

1. **Formatação** do disco (se escolheu formatar)
2. **Download** de pacotes (~500-800 MB)
3. **Instalação** de pacotes
4. **Configuração** do sistema
5. **Instalação** do bootloader

**Tempo:** 10-30 minutos (depende da internet)

---

### **6.2 Avisos Durante Instalação (NORMAIS!):**

Você vai ver MUITOS warnings amarelos:

```
WARNING: Possibly missing firmware for module: 'aic94xx'
WARNING: Possibly missing firmware for module: 'wd719x'
WARNING: Possibly missing firmware for module: 'xhci_pci'
zstd: error 70: Write error: cannot write block: Broken pipe
WARNING: consolefont: no font found in configuration
```

**✅ PODE IGNORAR!**
- São firmwares de hardware que você não tem
- Não afetam o sistema
- Arch mostra TODOS os avisos (outras distros escondem)

**❌ APENAS erros vermelhos (ERROR) são problemas!**

---

### **6.3 Conclusão da Instalação:**

Quando aparecer:
```
Installation finished. No error reported.
```

**🎉 SUCESSO!**

1. Escolha: **reboot** (ou digite `reboot`)
2. **Remova o pendrive** quando o PC desligar
3. O sistema vai reiniciar

---

## 🎊 7. Primeiro Boot

### **7.1 O que esperar:**

1. PC reinicia
2. Tela preta com **GRUB** aparece
3. Menu com opções:
   - **Arch Linux** ← Escolha esta!
   - Arch Linux (fallback)
   - (Se dual boot: Windows Boot Manager)
4. Pressione **ENTER** ou aguarde 5 segundos

### **7.2 Login:**

Tela de login aparece (pode ser texto ou gráfica):

```
archlinux login: SEU_USUARIO
Password: SUA_SENHA
```

### **7.3 Hyprland Inicia Automaticamente!**

Após login, o Hyprland deve iniciar mostrando:
- Papel de parede (pode ser logo do Hyprland)
- Barra no topo (Waybar)
- Mensagem amarela sobre configuração

**🎉 PARABÉNS! ARCH INSTALADO!**

---

## 📝 Checklist Final da Parte 1:

- ✅ ISO baixada e pendrive criado
- ✅ Secure Boot desabilitado
- ✅ Boot pelo pendrive funcionou
- ✅ Internet conectada
- ✅ archinstall configurado corretamente
- ✅ Instalação concluída sem erros
- ✅ Sistema iniciou e você fez login
- ✅ Hyprland está rodando

---

## ➡️ Próximos Passos:

**Continue na Parte 2 para:**
- Configuração pós-instalação
- Instalar yay (AUR helper)
- Configurar Hyprland detalhadamente
- Configurar Waybar
- Instalar programas essenciais
- Deixar o sistema usável no dia a dia

---

**Fim da Parte 1 de 3**

*Criado com base em experiência real de instalação, incluindo todos os problemas encontrados e soluções aplicadas.*