# 🏹 Arch Linux Desktop Agent Skill

[English](../../README.md) | [Português](../pt-br/README.md) | [Español](README.md)

[![License: MIT](https://img.shields.io/badge/Licencia-MIT-blue.svg)](../../LICENSE)
[![Platform](https://img.shields.io/badge/Plataforma-Arch%20Linux-1793d1?logo=arch-linux&logoColor=white)](https://archlinux.org)
[![Skill Standard](https://img.shields.io/badge/Agent%20Skill-v1.0-brightgreen.svg)](https://skills.sh)

Una **Agent Skill** modular y de nivel de producción que capacita a asistentes de IA (Gemini, Claude, Antigravity, Open Agent Skills) para configurar, personalizar temas, automatizar y diagnosticar sistemas de escritorio **Arch Linux** con la máxima precisión y seguridad.

Compatible de forma nativa con los principales **Compositores Wayland** y **Entornos de Escritorio (DEs)**.

---

## 💡 Inspiración y Créditos

Este proyecto se inspiró originalmente en la arquitectura pionera de skills de IA desarrollada por el proyecto [**Omarchy**](https://omarchy.org/) ([`basecamp/omarchy`](https://github.com/basecamp/omarchy)), una distribución Linux orientada a la inteligencia artificial.

Mientras que Omarchy utiliza capas y herramientas exclusivas de su distribución (configuraciones en Lua, barra en Quickshell y utilidades propietarias), esta skill extrae y generaliza esos principios de seguridad y automatización hacia el **estándar puro de upstream de Arch Linux**, compatible con Hyprland, Sway, i3, KDE Plasma 6, GNOME y Niri.

---

## 🚀 Características Principales

* **Soporte Multi-Entorno:** Guías de configuración nativa para **Hyprland**, **Sway**, **i3wm**, **KDE Plasma 6**, **GNOME** y **Niri**.
* **Seguridad Garantizada:** Pautas estrictas que impiden a la IA modificar o dañar archivos del sistema (`/usr/`, `/etc/`); restringe ediciones a `~/.config/`, realiza copias de seguridad automáticas antes de editar y gestiona privilegios de forma segura (`sudo` vs `pkexec`; nunca ejecuta `yay`/`paru` como root).
* **Pantallas y Escalado:** Detección automática de monitores, tasas de refresco y escalado fraccionario nítido.
* **Waybar y Widgets Listos:** Diseños en JSONC y estilos GTK CSS completos para Waybar, Rofi-wayland, SwayNC, Hypridle e Hyprlock.
* **Canalización de Temas Moderna:** Personalización coherente entre GTK 3/4, Qt5/Qt6 (Kvantum/qt6ct), generadores dinámicos de color (`matugen`, `pywal`) y emuladores de terminal (Ghostty, Kitty, Alacritty, Foot).
* **Captura de Pantalla y OCR:** Capturas nativas en Wayland (`grim`/`slurp`/`grimblast`), grabación acelerada por GPU (`wf-recorder`/`gpu-screen-recorder`) y extracción inmediata de texto con OCR (`tesseract`).
* **Mantenimiento del Sistema:** Mejores prácticas con `pacman`, gestores de AUR (`yay`, `paru`), optimización de mirrors con `reflector`, eliminación de paquetes huérfanos y diagnóstico del servidor de audio PipeWire.

---

## 📂 Estructura del Repositorio

```
.
├── SKILL.md                  # Punto de entrada del agente y reglas de seguridad
├── README.md                 # Documentación en Inglés
├── CONTRIBUTING.md           # Guía de contribución (Inglês)
├── LICENSE                   # Licencia MIT
├── .gitignore                # Archivos ignorados
│
├── docs/                     # Documentación Humana Multilingüe
│   ├── pt-br/                # 🇧🇷 Documentação em Português
│   │   ├── README.md
│   │   └── CONTRIBUTING.md
│   └── es/                   # 🇪🇸 Documentación en Español
│       ├── README.md
│       └── CONTRIBUTING.md
│
└── references/               # Base de Conocimiento del Agente
    ├── environments/         # Window Managers & Desktop Environments
    │   ├── hyprland.md       # Sintaxis nativa de Hyprland y window rules v2
    │   ├── sway-i3.md        # Mosaicos en Sway (Wayland) e i3 (X11)
    │   ├── kde-plasma.md     # Automatización CLI en KDE Plasma 6 y KWin
    │   ├── gnome.md          # Configuraciones de GNOME mediante gsettings y dconf
    │   └── niri.md           # Compositor de mosaico con desplazamiento infinito Niri
    │
    └── subsystems/           # Subsistemas de Arch Linux
        ├── bar-and-widgets.md # Waybar, Rofi, SwayNC, Hyprlock, Hypridle
        ├── theming.md         # Fondos de pantalla, GTK/Qt, Matugen, fuentes
        ├── hooks.md           # Listeners IPC de compositores, timers de systemd
        ├── capture.md         # Capturas, grabación, OCR y portales WebRTC
        └── packages-and-system.md # Pacman, AUR, reflector y drivers
```

---

## 📦 Instalación y Uso

### 1. Para Gemini / Antigravity CLI
Clona esta carpeta en tu directorio de skills:
```bash
git clone https://github.com/TU_USUARIO/arch-linux-skill.git ~/.gemini/config/skills/arch-linux
```

### 2. Para Claude Desktop / Code / Open Agent Skills
```bash
mkdir -p ~/.agents/skills
git clone https://github.com/TU_USUARIO/arch-linux-skill.git ~/.agents/skills/arch-linux
```

### 3. Mediante Skills CLI (`skills.sh`)
```bash
npx skills add TU_USUARIO/arch-linux-skill
```

---

## 💡 Ejemplos de Peticiones (Prompts)

Una vez instalada, tu agente de IA activará esta skill automáticamente al solicitar:

* *"Agrega el atajo Super+E para abrir Nautilus en Hyprland"*
* *"Cambia el estilo de mi Waybar a un tema flotante Catppuccin Mocha"*
* *"Configura mi monitor externo de 144Hz en Sway"*
* *"¿Cómo activo el modo oscuro y cambio el tamaño de fuente en GNOME desde terminal?"*
* *"Crea un timer de systemd para rotar fondos de pantalla cada 30 minutos"*
* *"Configura extracción de texto (OCR) desde la pantalla con un atajo"*
* *"Limpia la caché de pacman y elimina paquetes huérfanos de forma segura"*

---

## 🛡️ Garantías de Seguridad

Esta skill impone reglas estrictas a los agentes de IA:
1. **Nunca editar archivos del sistema (`/usr/`, `/etc/`)** a menos que se solicite expresamente.
2. **Crear siempre copias de seguridad** (`*.bak.<timestamp>`) antes de realizar modificaciones.
3. **Nunca ejecutar gestores de AUR (`yay`, `paru`) con `sudo`**.
4. **Validar siempre las configuraciones** después de editarlas (`hyprctl reload`, `niri validate`, `swaymsg reload`).

---

## 🤝 Contribuciones

¡Las contribuciones son bienvenidas! Consulta [CONTRIBUTING.md](CONTRIBUTING.md) para más detalles.

---

## 📄 Licencia

Distribuido bajo la [Licencia MIT](../../LICENSE).
