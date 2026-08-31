# Contributing to Arch Linux Agent Skill

[English](CONTRIBUTING.md) | [Português](CONTRIBUTING.pt-br.md) | [Español](CONTRIBUTING.es.md)

Thank you for contributing to the **Arch Linux Multi-Environment Agent Skill**! 🎉

This repository provides open, standardized knowledge and safety boundaries for AI coding agents (Gemini, Claude, Antigravity, and Open Agent Skills) managing Arch Linux desktop systems.

---

## 🌟 Areas Where You Can Help

1. **Adding New Window Managers & Desktop Environments:**
   * Support for *River*, *bspwm*, *AwesomeWM*, *Cosmic Desktop*, *Qtile*, *XFCE*, etc.
2. **Updating Existing Topic Guides:**
   * Keeping up with upstream breaking changes in Hyprland, Waybar, KWin, or Mutter.
3. **Enhancing Safe Automation Hooks & Scripts:**
   * Systemd user services, udev automation, and compositor IPC listeners.
4. **Expanding Theming & UI Recipes:**
   * Popular palette templates (Catppuccin, Tokyo Night, Nord, Gruvbox) across terminals, bars, and GTK/Qt.

---

## 📐 Standards for Agent Skills

AI Agent Skills require concise, unambiguous, and deterministic documentation. When writing or updating guides:

### 1. Enforce Non-Destructive Safety Rules
* **User space first:** Instruct agents to write configurations strictly to `~/.config/` or `~/.local/bin/`.
* **Automated Backups:** Include backup steps (`cp file file.bak.$(date +%s)`) before any major configuration modification.
* **AUR Privilege Separation:** Always emphasize that AUR helpers (`yay`, `paru`) **must never be run with `sudo`**.

### 2. Upstream Purity
* Avoid proprietary or distro-specific wrappers.
* Use standard Arch Linux upstream tooling (`pacman`, `systemd --user`, native `.conf`/`.jsonc`/`.kdl` formats).

### 3. Clear Code Blocks & Syntax
* Provide copy-pasteable, verified CLI commands and configuration snippets with proper syntax tags (e.g. ````ini````, ````bash````, ````jsonc````, ````css````, ````kdl````).

---

## 📝 Guide Template for New Window Managers / DEs

If you are adding a new environment (e.g., `river.md` or `cosmic.md`), follow this structure:

```markdown
# [Environment Name] Guide (Arch Linux)

This guide covers configuring and managing **[Name]** on Arch Linux.

---

## 1. Configuration Locations
* Main config: `~/.config/[app]/...`
* Modular sub-configs (if applicable)

## 2. Display & Monitor Scaling
* How to query and set resolutions, refresh rates, and scales.

## 3. Core Keybindings & Window Rules
* Syntax for applications, window navigation, and floating rules.

## 4. Autostart Daemons & Status Bar Integration
* How to launch status bars (Waybar/Polybar), notifications, and idle daemons.

## 5. Validation & Reloading
* CLI commands to reload without restarting the session.
```

*Don't forget to link your new guide in [`SKILL.md`](SKILL.md) and [`README.md`](README.md).*

---

## 🛠️ Development & Submission Workflow

### 1. Fork & Branch
```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/arch-linux-skill.git
cd arch-linux-skill

# Create a topic branch
git checkout -b feat/add-river-wm
```

### 2. Commit Guidelines
We follow [Conventional Commits](https://www.conventionalcommits.org/):
* `feat:` A new desktop environment or major guide (e.g., `feat: add River compositor guide`)
* `fix:` Correcting an outdated command or syntax error (e.g., `fix: update hyprland windowrulev2 syntax`)
* `docs:` Documentation improvements in README or contributing guides
* `chore:` Repository maintenance, gitignore, or metadata updates

### 3. Submitting a Pull Request
1. Ensure Markdown links are functional and relative.
2. Push your branch to GitHub (`git push origin feat/add-river-wm`).
3. Open a Pull Request against the `main` branch.
4. Describe what changed, why, and which desktop environment versions were tested.

---

## 💡 Acknowledgments

This skill was originally inspired by the agent skills architecture of the [Omarchy](https://omarchy.org/) project. We aim to maintain that same standard of safety, clarity, and excellence for the wider Arch Linux community.
