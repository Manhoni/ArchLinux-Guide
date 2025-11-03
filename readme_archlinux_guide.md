# 📘 Arch Linux + Hyprland Complete Installation Guide

A comprehensive, step-by-step guide for installing and configuring Arch Linux with Hyprland compositor, based on real installation experience with complete troubleshooting.

[![Arch Linux](https://img.shields.io/badge/Arch%20Linux-1793D1?style=for-the-badge&logo=arch-linux&logoColor=white)](https://archlinux.org/)
[![Hyprland](https://img.shields.io/badge/Hyprland-58E1FF?style=for-the-badge&logo=wayland&logoColor=black)](https://hyprland.org/)
[![Wayland](https://img.shields.io/badge/Wayland-FFBB00?style=for-the-badge&logo=wayland&logoColor=black)](https://wayland.freedesktop.org/)

---

## 📚 Guide Structure

This guide is split into **3 comprehensive parts** covering the entire installation process, post-installation configuration, and troubleshooting:

### 📄 [Part 1: Preparation & Installation](arch_guide_part1of3.md)
**What's Covered:**
- Hardware requirements and compatibility
- Downloading Arch Linux ISO
- Creating bootable USB drive (Linux/Windows/macOS)
- BIOS/UEFI configuration (Secure Boot, UEFI mode)
- Complete installation process with `archinstall`
- Step-by-step configuration options
- First boot and login

**Target Audience:** Beginners to intermediate users

---

### 📄 [Part 2: Post-Installation & Configuration](arch_guide_part2of3.md)
**What's Covered:**
- Post-installation system updates
- **Complete Wi-Fi setup guide** (3 methods: nmcli, nmtui, iwctl)
- Installing and configuring yay (AUR helper)
- **Complete Hyprland configuration** with keybindings
- Waybar setup (status bar) with custom CSS
- Essential packages for daily use
- Terminal configuration (Foot, Kitty, Alacritty)

**Highlights:**
- 🌐 Detailed Wi-Fi connection guide (perfect for classroom/mobile hotspot)
- ⌨️ Brazilian ABNT2 keyboard configuration
- 🎨 Customizable Waybar with modern design

---

### 📄 [Part 3: Troubleshooting & Advanced Setup](arch_guide_part3of3.md)
**What's Covered:**
- **Common problems and solutions** (terminal won't open, wrong keyboard layout, etc.)
- Dual boot with Windows 11 (complete guide)
- Important Hyprland commands (`hyprctl`)
- **Backup and restore scripts** (portable between machines)
- Performance optimizations
- Bluetooth management
- Database setup (MariaDB + phpMyAdmin)
- PDF viewer (Zathura) and image viewer (imv) configuration

**Special Sections:**
- 🔧 Emergency recovery procedures
- 💾 Complete backup/restore system
- 🎯 Keyboard shortcuts reference
- 🐛 Common issues with VirtualBox/VMware

---

## 🎯 What Makes This Guide Different?

### ✅ Real-World Experience
- Based on actual installation with all problems encountered and solved
- Not just theory - includes **real errors** and their solutions
- Covers issues that official documentation doesn't mention

### ✅ Beginner-Friendly
- Every step explained in detail
- Screenshots and examples included
- No assumed prior knowledge
- Portuguese → English translation available

### ✅ Comprehensive Troubleshooting
- Terminal not opening? Covered.
- Keyboard layout wrong? Fixed.
- Bluetooth not working? Solved.
- Forgot password? We got you.

### ✅ Optimized for Different Hardware
- Intel integrated graphics (tested)
- Works on both VMs and bare metal
- Dual boot instructions included
- Laptop-specific configurations (battery, brightness, touchpad)

---

## 🖥️ Tested Hardware Configuration

This guide was tested on:
- **CPU:** Intel (2 cores / 4 threads)
- **RAM:** 12 GB
- **GPU:** Intel integrated graphics
- **Storage:** SSD (recommended)

**Also tested on:**
- VirtualBox (with workarounds)
- VMware Workstation
- Dual boot with Windows 11

---

## 📦 What You'll Get After Following This Guide

### Core System
- ✅ Arch Linux (rolling release, always up-to-date)
- ✅ Hyprland compositor (modern Wayland WM)
- ✅ Waybar (customizable status bar)
- ✅ NetworkManager (easy Wi-Fi management)

### Essential Applications
- 🖥️ **Terminals:** Foot, Kitty, Alacritty
- 🌐 **Browser:** Firefox
- 📁 **File Manager:** Thunar (lightweight)
- 📄 **PDF Viewer:** Zathura (themeable)
- 🖼️ **Image Viewer:** imv (Wayland native)
- 📝 **Text Editors:** Neovim, Nano
- 🎨 **Screenshots:** grim + slurp

### Development Tools
- Git, base-devel
- yay (AUR helper)
- MariaDB + phpMyAdmin (optional)

### Multimedia
- PipeWire (audio)
- Bluetooth support
- Hardware acceleration (Intel)

---

## 🚀 Quick Start

### For Beginners (Start Here!)

1. **Read Part 1** - Understand the process before starting
2. **Prepare USB drive** - Follow the bootable USB creation guide
3. **Disable Secure Boot** - Critical step explained in Part 1
4. **Follow installation** - Use `archinstall` with our recommended settings
5. **Configure post-installation** - Part 2 has everything you need

### For Advanced Users

Jump to specific sections:
- Wi-Fi setup → Part 2, Section 2
- Dual boot → Part 3, Section 2
- Troubleshooting → Part 3, Section 1
- Backup scripts → Part 3, Section 4

---

## ⚠️ Important Notes

### Before You Start

- ⚠️ **Backup your data!** Installation will format the selected disk
- 📖 Read the entire part before starting each section
- 🕐 Set aside 2-3 hours for the full installation
- 🌐 Stable internet connection required
- 💾 Minimum 20 GB free disk space (50 GB recommended)

### Known Issues

- **VirtualBox:** Limited 3D acceleration affects performance
  - Solution: Use VMware or bare metal
- **Kitty terminal:** May not work in VMs
  - Solution: Use Foot or Alacritty instead
- **Btrfs subvolumes:** Can cause mounting issues
  - Solution: Use ext4 filesystem (recommended in guide)

---

## 🛠️ Troubleshooting Quick Links

| Problem | Solution Location |
|---------|-------------------|
| Terminal won't open | Part 3, Section 1.1 |
| Keyboard layout wrong | Part 3, Section 1.2 |
| Resolution too large | Part 3, Section 1.3 |
| Forgot password | Part 3, Section 1.4 |
| Hyprland won't start | Part 3, Section 1.5 |
| No audio | Part 3, Section 1.6 |
| Bluetooth issues | Part 3, Section 1.7 |
| Battery drain | Part 3, Section 1.8 |
| VM performance issues | Part 3, Section 1.9 |

---

## 📖 Additional Resources

### Official Documentation
- [Arch Wiki](https://wiki.archlinux.org/) - The best Linux documentation
- [Hyprland Wiki](https://wiki.hyprland.org/) - Official Hyprland docs
- [Wayland Documentation](https://wayland.freedesktop.org/)

### Community
- [Arch Linux Forums](https://bbs.archlinux.org/)
- [Reddit r/archlinux](https://reddit.com/r/archlinux)
- [Reddit r/hyprland](https://reddit.com/r/hyprland)

### Dotfiles & Inspiration
- [Awesome Hyprland](https://github.com/hyprland-community/awesome-hyprland)
- Search "hyprland dotfiles" on GitHub

---

## 📝 Guide Features

### Part 1 Highlights
- 🔧 Complete BIOS configuration
- 💿 Bootable USB creation (all platforms)
- 📋 `archinstall` detailed walkthrough
- ⚙️ All configuration options explained
- 🎯 Hardware-specific recommendations

### Part 2 Highlights
- 🌐 **3 Wi-Fi connection methods** (nmcli, nmtui, iwctl)
- 📡 Mobile hotspot connection guide
- 🎨 Complete Hyprland config file
- 🎯 Custom keybindings
- 💻 Terminal emulator comparison
- 🖼️ Waybar with custom CSS themes

### Part 3 Highlights
- 🐛 **All common problems solved**
- 💻 Windows 11 dual boot guide
- 💾 Backup/restore scripts
- 🔥 Performance optimizations
- 🎨 Theme customization
- 📱 Bluetooth management
- 🗄️ Database setup (LAMP stack)

---

## 🎨 Customization Topics Covered

- **Themes:** GTK themes, icon packs, cursors
- **Colors:** Synthwave/Miami Vice color schemes
- **Fonts:** Nerd Fonts installation
- **Waybar:** Complete customization guide
- **Terminal:** Color schemes for all terminals
- **Hyprland:** Borders, animations, opacity

---

## 💡 Pro Tips Included

- Using `F4` in file managers for integrated terminal
- `hyprctl` commands for debugging
- Package management with `yay`
- Keyboard shortcuts reference
- System monitoring tools
- Power management for laptops
- Bluetooth quick connection scripts

---

## 🔄 Updates & Maintenance

This guide covers:
- Regular system updates (`yay -Syu`)
- Cleaning package cache
- Managing orphaned packages
- Backup strategies
- Recovery procedures

---

## 🤝 Contributing

Found an error? Have a suggestion? Want to add content?

Feel free to:
- Open an issue
- Submit a pull request
- Share your experience
- Suggest improvements

---

## 📄 License

This guide is provided as-is for educational purposes. Feel free to use, modify, and share.

---

## 🙏 Acknowledgments

- Arch Linux community for excellent documentation
- Hyprland developers for an amazing compositor
- Everyone who contributed to the tools mentioned in this guide

---

## ⭐ Support

If this guide helped you, please:
- ⭐ Star this repository
- 🔄 Share with others
- 💬 Provide feedback
- 🐛 Report issues

---

## 📞 Getting Help

**Before asking for help:**
1. Check the troubleshooting section (Part 3, Section 1)
2. Search the Arch Wiki
3. Review the error messages

**Where to ask:**
- Arch Linux Forums
- Reddit r/archlinux
- Hyprland Discord

**When asking, include:**
- Which part/section you're on
- Exact error message
- Hardware specs
- What you've already tried

---

## 🚀 Next Steps After Installation

1. ✅ Follow the post-installation guide (Part 2)
2. ✅ Install your favorite applications
3. ✅ Customize themes and colors
4. ✅ Set up backups
5. ✅ Join the Arch community
6. ✅ Share your experience!

---

**Happy Arch Linux + Hyprland journey! 🐧✨**

*Last updated: 2025*
*Based on: Arch Linux 2025.10.01 with Kernel 6.16.8*