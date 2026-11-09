---
name: arch-linux
description: >
  REQUIRED for managing, configuring, customizing, and troubleshooting Arch Linux desktop systems.
  Use when working with Window Managers and Desktop Environments: Hyprland (~/.config/hypr/),
  Sway & i3 (~/.config/sway/, ~/.config/i3/), KDE Plasma 6 (kwriteconfig6, kdeglobals, kwinrc),
  GNOME (gsettings, dconf), and Niri (~/.config/niri/). Also covers status bars (Waybar, Polybar),
  app launchers (Rofi, Wofi), theming (GTK, Qt, wallpapers via swww/hyprpaper), audio (PipeWire),
  screen capture/sharing (grim, wf-recorder, localsend), package management (pacman/yay/paru),
  and crash diagnostics (coredumpctl, debuginfod, OOM triage).
---

# Arch Linux Multi-Environment Skill

Manage, configure, and customize **Arch Linux** desktop systems across all major Window Managers (WMs) and Desktop Environments (DEs).

*Inspired by the agent skill design principles of the [Omarchy](https://omarchy.org/) project, generalized for upstream Arch Linux.*

This skill guides configuration, theming, window management, package maintenance, display settings, and system components on Arch Linux.

---

## When This Skill MUST Be Used

**Invoke this skill for user requests involving ANY of these on Arch Linux:**

- **Hyprland:** Editing `~/.config/hypr/` (`hyprland.conf`, `monitors.conf`, `keybindings.conf`, `windowrules.conf`, `looknfeel.conf`, `hypridle.conf`, `hyprlock.conf`)
- **Sway & i3wm:** Editing `~/.config/sway/config` or `~/.config/i3/config`, `swaymsg`, `i3-msg`
- **KDE Plasma 6:** Configuring `kwriteconfig6`, `kwinrc`, `kdeglobals`, shortcuts, KWin rules, and Plasma themes
- **GNOME:** Configuring `gsettings`, `dconf`, GNOME extensions (`gnome-extensions`), Mutter keybindings, and dynamic workspaces
- **Niri:** Configuring `~/.config/niri/config.kdl` and `niri msg`
- **Status Bars & Launchers:** `~/.config/waybar/`, Polybar, Rofi (`config.rasi`), Wofi, SwayNC, Dunst, Mako
- **Theming:** GTK 3/4, Qt5/Qt6 (Kvantum, qt6ct), wallpapers (`swww`, `hyprpaper`, `waypaper`), color generators (`matugen`, `pywal`), and fontconfig
- **Terminals:** Ghostty, Kitty, Alacritty, Foot
- **Screen Capture & Portals:** `grim`, `slurp`, `grimblast`, `wf-recorder`, `gpu-screen-recorder`, `xdg-desktop-portal`, `localsend`, `tailscale`
- **Package Management & System:** `pacman`, AUR helpers (`yay`, `paru`), mirrorlist (`reflector`), system updates, and cache cleaning
- **Crash Diagnostics:** `coredumpctl`, `gdb` with Arch `debuginfod`, OOM triage, and system journal inspection

---

## Topic Guides

Detailed guides for specific desktop environments and subsystems:

### Desktop Environments & Window Managers
- [`references/environments/hyprland.md`](references/environments/hyprland.md) — Hyprland native config, monitors, window rules v2, hyprsunset, and `hyprctl`
- [`references/environments/sway-i3.md`](references/environments/sway-i3.md) — Sway (Wayland) and i3 (X11) configuration, outputs, workspaces, and `swaymsg`
- [`references/environments/kde-plasma.md`](references/environments/kde-plasma.md) — KDE Plasma 6 CLI tools (`kwriteconfig6`), KWin rules, themes, and shortcuts
- [`references/environments/gnome.md`](references/environments/gnome.md) — GNOME configuration via `gsettings`, `dconf`, Mutter, and GNOME extensions
- [`references/environments/niri.md`](references/environments/niri.md) — Niri horizontal scrollable tiling compositor (`config.kdl`) and `niri msg`

### System Subsystems & Customization
- [`references/subsystems/bar-and-widgets.md`](references/subsystems/bar-and-widgets.md) — Waybar layout, CSS styling, custom modules, SwayNC, Hypridle, and Hyprlock
- [`references/subsystems/theming.md`](references/subsystems/theming.md) — Wallpapers (`swww`, `hyprpaper`), GTK/Qt themes, color palettes, terminal themes, and fonts
- [`references/subsystems/hooks.md`](references/subsystems/hooks.md) — Compositor IPC event listeners, systemd user services/timers, and udev rules
- [`references/subsystems/capture.md`](references/subsystems/capture.md) — Screenshots, screen recording, OCR text capture, LocalSend/Tailscale file transfer
- [`references/subsystems/diagnostics-and-crash.md`](references/subsystems/diagnostics-and-crash.md) — Application crashes, coredumpctl, Debuginfod symbolization, and OOM analysis
- [`references/subsystems/packages-and-system.md`](references/subsystems/packages-and-system.md) — `pacman`, AUR helpers (`yay`/`paru`), system updates, driver troubleshooting, and logs

---

## Critical Safety Rules & Best Practices

1. **User Space First:** Always write user configuration files in `~/.config/` or `~/.local/bin/`.
2. **Backup Before Changes:** Make a timestamped backup before modifying critical files:
   ```bash
   cp ~/.config/hypr/hyprland.conf ~/.config/hypr/hyprland.conf.bak.$(date +%s)
   ```
3. **Privilege Escalation:**
   * Use `sudo` for administrative actions (`pacman`, system systemd units).
   * **NEVER run AUR helpers (`yay`, `paru`) with `sudo`!** AUR helpers manage build directories as a normal user.
4. **Validation:**
   * Hyprland: `hyprctl reload` and `hyprctl configerrors`
   * Niri: `niri validate`
   * Sway: `swaymsg reload`
5. **Memory & Core Dump Privacy:**
   * Core dumps contain memory snapshots with potential plain-text secrets (passwords, tokens).
   * Always extract to ephemeral `mktemp` paths and delete immediately via `trap`.
   * Never commit or upload core dumps to public issue trackers.
6. **Untrusted Themes & Scripts:**
   * Do not run arbitrary executable scripts or binaries packaged inside downloaded themes or dotfiles.

---

## Decision Framework

When handling user requests on Arch Linux:

1. **Identify the Environment:** Check running session (`echo $XDG_CURRENT_DESKTOP`, `echo $XDG_SESSION_TYPE`, `pgrep -l "hyprland|sway|plasma|gnome|niri"`).
2. **Apply the Right Guide:**
   - Hyprland -> [`references/environments/hyprland.md`](references/environments/hyprland.md)
   - Sway / i3 -> [`references/environments/sway-i3.md`](references/environments/sway-i3.md)
   - KDE Plasma -> [`references/environments/kde-plasma.md`](references/environments/kde-plasma.md)
   - GNOME -> [`references/environments/gnome.md`](references/environments/gnome.md)
   - Niri -> [`references/environments/niri.md`](references/environments/niri.md)
3. **Package Installation:** Use `pacman -S` for official repos; `yay -S` / `paru -S` without `sudo` for AUR.
4. **Service & Audio Management:** Use `systemctl --user` for user-space daemons (Waybar, PipeWire, SwayNC, Dunst, etc.).
5. **Crash & Diagnostics:** Follow [`references/subsystems/diagnostics-and-crash.md`](references/subsystems/diagnostics-and-crash.md): check `coredumpctl info`, rule out OOM kills, symbolize via Debuginfod, and securely clean up core files.
