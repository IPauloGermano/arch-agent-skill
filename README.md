# 🏹 Arch Linux Desktop Agent Skill

[English](README.md) | [Português](docs/pt-br/README.md) | [Español](docs/es/README.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Arch%20Linux-1793d1?logo=arch-linux&logoColor=white)](https://archlinux.org)
[![Skill Standard](https://img.shields.io/badge/Agent%20Skill-v1.0-brightgreen.svg)](https://skills.sh)

A modular, production-grade **Agent Skill** that empowers AI coding assistants (Gemini, Claude, Antigravity, Open Agent Skills) to configure, theme, automate, and troubleshoot **Arch Linux** desktop systems safely and accurately.

Supports all major **Wayland Compositors** and **Desktop Environments** out of the box.

---

## 💡 Inspiration & Credits

This project was originally inspired by the pioneering agent skills developed by the [**Omarchy**](https://omarchy.org/) project ([`basecamp/omarchy`](https://github.com/basecamp/omarchy)), an AI-first Linux distribution. 

While Omarchy is built with opinionated distribution wrappers (Lua configurations, Quickshell status bar, and custom CLI tools), this skill extracts and generalizes those agent safety principles into a **pure, multi-environment, upstream Arch Linux standard** compatible with Hyprland, Sway, i3, KDE Plasma 6, GNOME, and Niri.

---

## 🚀 Features

* **Multi-Environment Support:** Full native configuration guides for **Hyprland**, **Sway**, **i3wm**, **KDE Plasma 6**, **GNOME**, and **Niri**.
* **Safety First:** Strict guidelines against damaging system-level files; enforces user-space configs (`~/.config/`), automated backups before edits, and proper privilege escalation (`sudo` vs `pkexec`; prevents running `yay`/`paru` as root).
* **Display & Scaling:** Automated monitor detection, refresh rate configuration, and fractional scaling without blurry XWayland apps.
* **Waybar & UI Widgets:** Ready-to-use JSONC and GTK CSS layouts for Waybar, Rofi-wayland, SwayNC, Hypridle, and Hyprlock.
* **Modern Theming Pipeline:** Unified styling across GTK 3/4, Qt5/Qt6 (Kvantum/qt6ct), dynamic palette generators (`matugen`, `pywal`), and terminal emulators (Ghostty, Kitty, Alacritty, Foot).
* **Screen Capture & OCR:** Wayland-native screenshots (`grim`/`slurp`/`grimblast`), GPU-accelerated screen recording (`wf-recorder`/`gpu-screen-recorder`), and instant OCR text extraction (`tesseract`).
* **System Maintenance:** Best practices for `pacman`, AUR helpers (`yay`, `paru`), mirrorlist optimization (`reflector`), orphan package removal, and PipeWire audio health checks.

---

## 📂 Repository Structure

```
.
├── SKILL.md                  # Main entry point & safety rules for agents
├── README.md                 # English documentation
├── CONTRIBUTING.md           # Contribution guidelines (English)
├── LICENSE                   # MIT License
├── .gitignore                # Ignored files
│
├── docs/                     # Multilingual Human Documentation
│   ├── pt-br/                # 🇧🇷 Documentação em Português
│   │   ├── README.md
│   │   └── CONTRIBUTING.md
│   └── es/                   # 🇪🇸 Documentación en Español
│       ├── README.md
│       └── CONTRIBUTING.md
│
└── references/               # Agent Reference Knowledge Base
    ├── environments/         # Window Managers & Desktop Environments
    │   ├── hyprland.md       # Hyprland native syntax & window rules v2
    │   ├── sway-i3.md        # Sway (Wayland) & i3 (X11) tiling
    │   ├── kde-plasma.md     # KDE Plasma 6 CLI & KWin automation
    │   ├── gnome.md          # GNOME gsettings, dconf & extensions
    │   └── niri.md           # Niri scrollable tiling compositor
    │
    └── subsystems/           # System Subsystems
        ├── bar-and-widgets.md # Waybar, Rofi, SwayNC, Hyprlock, Hypridle
        ├── theming.md         # Wallpapers, GTK/Qt, Matugen, fonts
        ├── hooks.md           # Compositor IPC listeners, systemd timers
        ├── capture.md         # Screenshots, recording, OCR & portals
        └── packages-and-system.md # Pacman, AUR, reflector & drivers
```

---

## 📦 Installation & Usage

### 1. For Gemini / Antigravity CLI
Clone or copy this folder into your agent skills directory:
```bash
git clone https://github.com/YOUR_USERNAME/arch-linux-skill.git ~/.gemini/config/skills/arch-linux
```

### 2. For Claude Desktop / Code / Open Agent Skills
```bash
mkdir -p ~/.agents/skills
git clone https://github.com/YOUR_USERNAME/arch-linux-skill.git ~/.agents/skills/arch-linux
```

### 3. Using the Skills CLI (`skills.sh`)
```bash
npx skills add YOUR_USERNAME/arch-linux-skill
```

---

## 💡 Example Prompt Triggers

Once installed, your AI agent will automatically activate this skill when you ask:

* *"Add a keybinding Super+E to open Nautilus in Hyprland"*
* *"Change my Waybar styling to a floating Catppuccin Mocha theme"*
* *"Configure my external 144Hz monitor in Sway"*
* *"How do I enable dark mode and change font size in GNOME using CLI?"*
* *"Write a systemd user timer to rotate wallpapers every 30 minutes"*
* *"Set up OCR text extraction from screen with a shortcut"*
* *"Clean up pacman cache and remove orphan packages safely"*

---

## 🛡️ Safety Guarantees

This skill enforces critical rules for agents:
1. **Never edit files in system partitions (`/usr/`, `/etc/`)** unless explicitly requested by the user.
2. **Always backup** config files (`*.bak.<timestamp>`) before performing edits.
3. **Never run AUR helpers (`yay`, `paru`) with `sudo`**.
4. **Always validate configurations** after editing (`hyprctl reload`, `niri validate`, `swaymsg reload`).

---

## 🤝 Contributing

Contributions are welcome! Please check out [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding new environments.

---

## 📄 License

Distributed under the [MIT License](LICENSE).
