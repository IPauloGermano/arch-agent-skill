# 🏹 Arch Linux Desktop Agent Skill

[English](../../README.md) | [Português](README.md) | [Español](../es/README.md)

[![License: MIT](https://img.shields.io/badge/Licen%C3%A7a-MIT-blue.svg)](../../LICENSE)
[![Platform](https://img.shields.io/badge/Plataforma-Arch%20Linux-1793d1?logo=arch-linux&logoColor=white)](https://archlinux.org)
[![Skill Standard](https://img.shields.io/badge/Agent%20Skill-v1.0-brightgreen.svg)](https://skills.sh)

Uma **Agent Skill** modular de nível de produção que capacita assistentes de IA (Gemini, Claude, Antigravity, Open Agent Skills) a configurar, tematizar, automatizar e diagnosticar sistemas desktop **Arch Linux** com máxima precisão e segurança.

Compatível com os principais **Compositores Wayland** e **Ambientes de Desktop (DEs)** nativamente.

---

## 💡 Inspiração e Créditos

Este projeto foi originalmente inspirado na arquitetura pioneira de skills de IA desenvolvida pelo projeto [**Omarchy**](https://omarchy.org/) ([`basecamp/omarchy`](https://github.com/basecamp/omarchy)), uma distribuição Linux com foco em inteligência artificial.

Enquanto o Omarchy utiliza camadas e scripts de conveniência próprios (configurações em Lua, barra em Quickshell e comandos utilitários exclusivos), esta skill extrai e generaliza esses princípios de segurança e automação para o **padrão puro upstream do Arch Linux**, compatível com Hyprland, Sway, i3, KDE Plasma 6, GNOME e Niri.

---

## 🚀 Recursos e Capacidades

* **Suporte Multi-Ambiente:** Guias de configuração nativa para **Hyprland**, **Sway**, **i3wm**, **KDE Plasma 6**, **GNOME** e **Niri**.
* **Segurança em Primeiro Lugar:** Diretrizes rígidas que impedem a IA de corromper arquivos do sistema (`/usr/`, `/etc/`); força edições apenas em `~/.config/`, backups automáticos antes de alterações e separação de privilégios (`sudo` vs `pkexec`; nunca executa `yay`/`paru` como root).
* **Monitores e Escala:** Detecção automática de telas, taxas de atualização e escala fracionária otimizada.
* **Waybar e Widgets Prontos:** Layouts JSONC e temas GTK CSS completos para Waybar, Rofi-wayland, SwayNC, Hypridle e Hyprlock.
* **Pipeline de Tematização Moderno:** Estilização unificada entre GTK 3/4, Qt5/Qt6 (Kvantum/qt6ct), geradores dinâmicos de paleta (`matugen`, `pywal`) e terminais (Ghostty, Kitty, Alacritty, Foot).
* **Captura de Tela e OCR:** Screenshots Wayland nativos (`grim`/`slurp`/`grimblast`), gravação acelerada por GPU (`wf-recorder`/`gpu-screen-recorder`) e extração de texto direto da tela com OCR (`tesseract`).
* **Manutenção do Sistema:** Boas práticas com `pacman`, helpers do AUR (`yay`, `paru`), otimização de espelhos com `reflector`, remoção de órfãos e diagnóstico de áudio PipeWire.

---

## 📂 Estrutura do Repositório

```
.
├── SKILL.md                  # Ponto de entrada do agente e regras de segurança
├── README.md                 # Documentação em Inglês
├── CONTRIBUTING.md           # Guia de contribuição (Inglês)
├── LICENSE                   # Licença MIT
├── .gitignore                # Arquivos ignorados
│
├── docs/                     # Documentação Humana Multilíngue
│   ├── pt-br/                # 🇧🇷 Documentação em Português
│   │   ├── README.md
│   │   └── CONTRIBUTING.md
│   └── es/                   # 🇪🇸 Documentación en Español
│       ├── README.md
│       └── CONTRIBUTING.md
│
└── references/               # Base de Conhecimento do Agente
    ├── environments/         # Window Managers & Desktop Environments
    │   ├── hyprland.md       # Sintaxe nativa do Hyprland e window rules v2
    │   ├── sway-i3.md        # Tiling no Sway (Wayland) e i3 (X11)
    │   ├── kde-plasma.md     # Automação CLI no KDE Plasma 6 e KWin
    │   ├── gnome.md          # Configurações do GNOME via gsettings e dconf
    │   └── niri.md           # Compositor com rolagem infinita Niri
    │
    └── subsystems/           # Subsistemas do Arch Linux
        ├── bar-and-widgets.md # Waybar, Rofi, SwayNC, Hyprlock, Hypridle
        ├── theming.md         # Wallpapers, GTK/Qt, Matugen, fontes
        ├── hooks.md           # Listeners IPC de compositores, timers systemd
        ├── capture.md         # Screenshots, gravação, OCR e portais
        └── packages-and-system.md # Pacman, AUR, reflector e drivers
```

---

## 📦 Instalação e Uso

### 1. Para Gemini / Antigravity CLI
Clone a pasta para o diretório de skills do assistente:
```bash
git clone https://github.com/SEU_USUARIO/arch-linux-skill.git ~/.gemini/config/skills/arch-linux
```

### 2. Para Claude Desktop / Code / Open Agent Skills
```bash
mkdir -p ~/.agents/skills
git clone https://github.com/SEU_USUARIO/arch-linux-skill.git ~/.agents/skills/arch-linux
```

### 3. Usando a CLI do Skills (`skills.sh`)
```bash
npx skills add SEU_USUARIO/arch-linux-skill
```

---

## 💡 Exemplos de Prompts

Após instalado, seu assistente de IA ativará esta skill automaticamente quando você pedir:

* *"Adicione um atalho Super+E para abrir o Nautilus no Hyprland"*
* *"Mude o estilo da minha Waybar para um tema flutuante Catppuccin Mocha"*
* *"Configure meu monitor externo de 144Hz no Sway"*
* *"Como ativo o modo escuro e aumento a fonte no GNOME via terminal?"*
* *"Crie um timer do systemd para trocar de wallpaper a cada 30 minutos"*
* *"Configure extração de texto (OCR) da tela com um atalho"*
* *"Limpe o cache do pacman e remova pacotes órfãos com segurança"*

---

## 🛡️ Garantias de Segurança

Esta skill impõe regras rígidas para os agentes de IA:
1. **Nunca editar arquivos de sistema (`/usr/`, `/etc/`)** a menos que expressamente solicitado.
2. **Sempre criar backups** de arquivos (`*.bak.<timestamp>`) antes de modificá-los.
3. **Nunca executar helpers do AUR (`yay`, `paru`) com `sudo`**.
4. **Sempre validar configurações** após salvar (`hyprctl reload`, `niri validate`, `swaymsg reload`).

---

## 🤝 Contribuições

Contribuições são muito bem-vindas! Consulte o arquivo [CONTRIBUTING.md](CONTRIBUTING.md) para detalhes.

---

## 📄 Licença

Distribuído sob a [Licença MIT](../../LICENSE).
