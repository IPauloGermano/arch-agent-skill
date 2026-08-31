# GNOME Desktop Guide (Arch Linux)

This guide covers automating, configuring, and managing the **GNOME** desktop environment on Arch Linux using `gsettings` and `dconf`.

---

## 1. Architecture and Configuration (GSettings & Dconf)

GNOME manages its configurations via the GSettings database backed by `dconf`.

* **CLI configuration:** `gsettings`
* **Backup/Dump all user settings:** `dconf dump / > ~/gnome-backup.dconf`
* **Restore settings:** `dconf load / < ~/gnome-backup.dconf`

---

## 2. Window Management, Workspace & Keybinding Settings

### Shortcuts:
```bash
# Window close (Super+Q or Alt+F4)
gsettings set org.gnome.desktop.wm.keybindings close "['<Super>q', '<Alt>F4']"

# Maximize / Unmaximize
gsettings set org.gnome.desktop.wm.keybindings toggle-maximized "['<Super>f']"

# Switch workspaces
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-1 "['<Super>1']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-2 "['<Super>2']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-3 "['<Super>3']"

# Move window to workspace
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-1 "['<Super><Shift>1']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-2 "['<Super><Shift>2']"

# Toggle Overview
gsettings set org.gnome.shell.keybindings toggle-overview "['<Super>Space']"
```

### Window Manager Behavior (Mutter):
```bash
# Window focus mode: 'click', 'sloppy' (focus follows mouse), or 'mouse'
gsettings set org.gnome.desktop.wm.preferences focus-mode 'click'

# Titlebar buttons (minimize, maximize, close)
gsettings set org.gnome.desktop.wm.preferences button-layout 'appmenu:minimize,maximize,close'

# Workspaces on primary display only (false = span across all monitors)
gsettings set org.gnome.mutter workspaces-only-on-primary false

# Dynamic vs Static workspaces
gsettings set org.gnome.mutter dynamic-workspaces true
```

---

## 3. Appearance & Theming

```bash
# Dark mode preference
gsettings set org.gnome.desktop.interface color-scheme 'prefer-dark'

# GTK & Icon themes
gsettings set org.gnome.desktop.interface gtk-theme 'Adwaita-dark'
gsettings set org.gnome.desktop.interface icon-theme 'Papirus-Dark'
gsettings set org.gnome.desktop.interface cursor-theme 'Bibata-Modern-Classic'
gsettings set org.gnome.desktop.interface cursor-size 24

# Set Wallpaper (Light & Dark mode variants)
gsettings set org.gnome.desktop.background picture-uri "file:///path/to/wallpaper.png"
gsettings set org.gnome.desktop.background picture-uri-dark "file:///path/to/wallpaper_dark.png"

# Font settings
gsettings set org.gnome.desktop.interface font-name 'Cantarell 11'
gsettings set org.gnome.desktop.interface monospace-font-name 'JetBrainsMono Nerd Font 10'
```

---

## 4. Power & Idle Management

```bash
# Screen turn-off timeout (seconds; 0 = never)
gsettings set org.gnome.desktop.session idle-delay 300

# Automatic suspend on AC power (seconds; 0 = never)
gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-ac-timeout 1800

# Automatic suspend on Battery
gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-battery-timeout 900
```

---

## 5. Extensions Management (`gnome-extensions`)

GNOME Extensions extend the shell (e.g. Dash to Dock, AppIndicator, Blur my Shell, Pop Shell for tiling).

```bash
# List installed extensions
gnome-extensions list

# Enable or disable an extension
gnome-extensions enable dash-to-dock@micxgx.gmail.com
gnome-extensions disable ubuntu-dock@ubuntu.com

# Inspect extension details
gnome-extensions info blur-my-shell@aunetx
```

### Pop Shell (Auto-tiling on GNOME)
To add tiling window manager functionality to GNOME:
```bash
sudo pacman -S gnome-shell-extension-pop-shell
gnome-extensions enable pop-shell@system76.com
```
