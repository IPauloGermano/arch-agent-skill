# Theming, Wallpapers, and Fonts Guide (Arch Linux)

This guide covers theming, appearance, wallpaper management, and typography on Arch Linux with Hyprland.

---

## 1. Wallpaper Management

### Option A: `swww` (Smooth transitions & animations)
Start daemon in `autostart.conf`:
```ini
exec-once = swww-daemon
```

Set wallpaper with transition effects:
```bash
swww img /path/to/wallpaper.png --transition-type wipe --transition-duration 2
```

### Option B: `hyprpaper` (Native Hyprland wallpaper utility)
Configuration file: `~/.config/hypr/hyprpaper.conf`
```ini
preload = ~/Pictures/wallpapers/neon_city.png
wallpaper = , ~/Pictures/wallpapers/neon_city.png
splash = false
ipc = on
```
Start daemon in `autostart.conf`:
```ini
exec-once = hyprpaper
```

Change wallpaper dynamically via IPC:
```bash
hyprctl hyprpaper preload "/path/to/new_wall.png"
hyprctl hyprpaper wallpaper ",/path/to/new_wall.png"
```

---

## 2. GTK Theming (GTK 3 & GTK 4)

On Hyprland (Wayland), apply GTK settings via `gsettings` and `~/.config/gtk-3.0/settings.ini`:

### Via `gsettings`:
```bash
# Theme and Dark mode
gsettings set org.gnome.desktop.interface gtk-theme 'Catppuccin-Mocha-Standard-Lavender-Dark'
gsettings set org.gnome.desktop.interface color-scheme 'prefer-dark'

# Icons and Cursor
gsettings set org.gnome.desktop.interface icon-theme 'Papirus-Dark'
gsettings set org.gnome.desktop.interface cursor-theme 'Bibata-Modern-Classic'
gsettings set org.gnome.desktop.interface cursor-size 24

# Fonts
gsettings set org.gnome.desktop.interface font-name 'Roboto 11'
gsettings set org.gnome.desktop.interface document-font-name 'Roboto 11'
gsettings set org.gnome.desktop.interface monospace-font-name 'JetBrainsMono Nerd Font 10'
```

### GUI Editor for GTK on Wayland:
Install and use `nwg-look` (`pacman -S nwg-look`).

---

## 3. Qt Theming (Qt5 & Qt6)

To ensure Qt applications follow your dark theme and match GTK:

1. Install theme engines:
   ```bash
   sudo pacman -S qt5ct qt6ct kvantum
   ```

2. Set environment variables in `/etc/environment` or `~/.config/hypr/hyprland.conf`:
   ```ini
   env = QT_QPA_PLATFORM,wayland;xcb
   env = QT_QPA_PLATFORMTHEME,qt6ct
   env = QT_AUTO_SCREEN_SCALE_FACTOR,1
   env = QT_WAYLAND_DISABLE_WINDOWDECORATION,1
   ```

3. Open `qt6ct` or `kvantummanager` to pick your preferred color palette and icon themes.

---

## 4. Color Palette Integration

### Matugen (Material You / Dynamic Colors)
Generate cohesive system colors across Waybar, terminals, and apps directly from wallpaper:
```bash
matugen image /path/to/wallpaper.png
```

### Pywal
```bash
wal -i /path/to/wallpaper.png
```

### Popular Static Color Themes
* **Catppuccin** (Mocha, Macchiato, Frappé, Latte)
* **Tokyo Night** (Night, Storm, Moon)
* **Nord**
* **Rose Pine** (Main, Moon, Dawn)
* **Gruvbox Material**

---

## 5. Terminal Theming

### Ghostty (`~/.config/ghostty/config`)
```ini
theme = catppuccin-mocha
font-family = "JetBrainsMono Nerd Font"
font-size = 12
window-padding-x = 12
window-padding-y = 12
background-opacity = 0.90
```

### Kitty (`~/.config/kitty/kitty.conf`)
```ini
font_family      JetBrainsMono Nerd Font
font_size        11.5
background_opacity 0.88
include current-theme.conf
```
*List/switch themes interactively:* `kitty +kitten themes`

### Alacritty (`~/.config/alacritty/alacritty.toml`)
```toml
[font]
size = 11.0

[font.normal]
family = "JetBrainsMono Nerd Font"

[window]
opacity = 0.90
padding = { x = 10, y = 10 }
```

---

## 6. Fonts and Fontconfig

### Recommended Packages:
```bash
sudo pacman -S ttf-jetbrains-mono-nerd ttf-fira-code noto-fonts noto-fonts-emoji noto-fonts-cjk
```

### Refresh Font Cache:
```bash
fc-cache -fv
```

### List Installed Fonts:
```bash
fc-list : family | sort -u | grep -i "nerd"
```
