# Niri Scrollable Tiling Compositor Guide (Arch Linux)

This guide covers configuring and managing **Niri**, an infinite horizontal scrollable tiling Wayland compositor, on Arch Linux.

---

## 1. Configuration Overview

Niri uses the KDL configuration format:
* Location: `~/.config/niri/config.kdl`

Validate configuration without restarting:
```bash
niri validate
```

---

## 2. Display / Outputs in Niri

```kdl
output "eDP-1" {
    mode "1920x1200@60.0"
    scale 1.0
    position x=0 y=0
}

output "HDMI-A-1" {
    mode "2560x1440@144.0"
    scale 1.0
    position x=1920 y=0
}
```

---

## 3. Core Keybindings (`config.kdl`)

```kdl
binds {
    Mod+Return { spawn "kitty"; }
    Mod+D { spawn "rofi" "-show" "drun"; }
    Mod+Q { close-window; }

    // Column / Window navigation
    Mod+Left  { focus-column-left; }
    Mod+Right { focus-column-right; }
    Mod+Down  { focus-window-down; }
    Mod+Up    { focus-window-up; }

    // Move columns horizontally
    Mod+Shift+Left  { move-column-left; }
    Mod+Shift+Right { move-column-right; }

    // Workspaces
    Mod+1 { focus-workspace 1; }
    Mod+2 { focus-workspace 2; }
    Mod+Shift+1 { move-window-to-workspace 1; }
    Mod+Shift+2 { move-window-to-workspace 2; }

    // Fullscreen / Expand column
    Mod+F { fullscreen-window; }
    Mod+W { maximize-column; }
    Mod+C { center-column; }

    // Quit Niri
    Mod+Shift+E { quit; }
}
```

---

## 4. Window Rules

```kdl
window-rule {
    match app-id=r#"^pavucontrol$"#
    open-floating true
}

window-rule {
    match title=r#"^Picture-in-Picture$"#
    open-floating true
}

window-rule {
    match app-id=r#"^kitty$"#
    opacity 0.92
}
```

---

## 5. Controlling Niri from CLI

Niri provides an IPC interface via `niri msg`:

```bash
# Query windows and focused column
niri msg windows
niri msg focused-window

# Trigger actions
niri msg action focus-column-right
niri msg action toggle-debug-tint
```
