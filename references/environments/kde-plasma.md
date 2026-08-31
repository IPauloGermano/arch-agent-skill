# KDE Plasma Desktop Guide (Arch Linux)

This guide covers automating, configuring, and troubleshooting **KDE Plasma 6** on Arch Linux (both Wayland and X11 sessions).

---

## 1. Architecture and Config Locations

KDE Plasma configurations are stored in `~/.config/` in INI-style `.kdeglobals` and `kwin*` files:

```
~/.config/
├── kdeglobals            # System-wide theme, colors, fonts, and icon settings
├── kwinrc                # Window manager (KWin), virtual desktops, compositing, effects
├── kglobalshortcutsrc    # Global keyboard shortcuts
├── kscreenlockerrc       # Lock screen configuration and timeouts
├── kcminputrc            # Mouse, touchpad, and keyboard hardware settings
├── plasmashellrc         # Desktop shell layout and panel configurations
└── plasma-org.kde.plasma.desktop-appletsrc # Widgets, panels, and system tray config
```

---

## 2. CLI Configuration Tools (`kwriteconfig6` & `kreadconfig6`)

Use `kwriteconfig6` to programmatically update KDE settings without opening GUI menus:

```bash
# General syntax:
# kwriteconfig6 --file <filename> --group <group> --key <key> <value>

# Example: Change window titlebar double click action to maximize
kwriteconfig6 --file kwinrc --group Windows --key TitlebarDoubleClickAction Maximize

# Example: Adjust animation speed (0 = instant, 1 = normal, 2 = slow)
kwriteconfig6 --file kdeglobals --group KDE --key AnimationDurationFactor 0.5

# Example: Set number of virtual desktops to 4
kwriteconfig6 --file kwinrc --group Desktops --key Number 4
```

---

## 3. Theming & Appearance via CLI

KDE ships dedicated CLI helpers for theme management:

```bash
# Apply global color scheme (BreezeDark, CatppuccinMocha, TokyoNight, etc.)
plasma-apply-colorscheme BreezeDark

# Apply wallpaper
plasma-apply-wallpaperimage /path/to/wallpaper.png

# Apply cursor theme and size
plasma-apply-cursortheme Bibata-Modern-Classic --size 24

# Apply look-and-feel global theme
lookandfeeltool -a org.kde.breezedark.desktop
```

---

## 4. Window Rules in KWin

KWin window rules can be inspected and defined in `~/.config/kwinrulesrc`:

```ini
[1]
Description=Float Pavucontrol
wmclass=pavucontrol
wmclassmatch=1
types=1
floating=true
floatingrule=2
```

Reload KWin rules:
```bash
qdbus org.kde.KWin /KWin reconfigure
```

---

## 5. Keyboard Shortcuts (`kglobalshortcutsrc`)

Inspect or edit global keybindings in `~/.config/kglobalshortcutsrc`:

```ini
[kwin]
Window Close=Alt+F4\tMeta+Q,Alt+F4,Close Window
Window Maximize=Meta+F,Meta+F,Maximize Window
Overview=Meta+W,Meta+W,Toggle Overview
```

Reload shortcuts daemon:
```bash
kquitapp6 kglobalaccel && kglobalaccel6 &
```

---

## 6. Restarting Plasma Shell & Diagnostics

```bash
# Restart plasmashell without logging out:
kquitapp6 plasmashell && kstart plasmashell

# Restart KWin on Wayland:
# Note: On Wayland, KWin is the display server, so restarting it ends the session.
# On X11:
kwin_x11 --replace &

# Check Plasma systemd logs:
journalctl --user -u plasma-plasmashell -b -e
```
