# Sway (Wayland) and i3 (X11) Guide (Arch Linux)

This guide covers configuring and managing **Sway** (Wayland) and **i3wm** (X11) on Arch Linux.

---

## 1. Configuration Locations

* **Sway (Wayland):** `~/.config/sway/config`
* **i3 (X11):** `~/.config/i3/config`

Both share the exact same syntax for window management, keybindings, and workspaces. Sway adds Wayland-specific features like `output` (monitors) and `input` (touchpads/keyboards).

---

## 2. Monitor & Display Configuration in Sway

```ini
# Auto detect
output * bg ~/Pictures/wallpapers/neon.png fill

# Specific display output
output eDP-1 resolution 1920x1200@60Hz position 0,0 scale 1.0
output HDMI-A-1 resolution 2560x1440@144Hz position 1920,0 scale 1.0

# Query connected outputs in Sway
# CLI: swaymsg -t get_outputs
```

---

## 3. Core Keybindings & Window Management

```ini
set $mod Mod4
set $term kitty
set $menu rofi -show drun

# Start programs
bindsym $mod+Return exec $term
bindsym $mod+d exec $menu
bindsym $mod+Shift+q kill

# Change focus
bindsym $mod+h focus left
bindsym $mod+j focus down
bindsym $mod+k focus up
bindsym $mod+l focus right

# Move focused window
bindsym $mod+Shift+h move left
bindsym $mod+Shift+j move down
bindsym $mod+Shift+k move up
bindsym $mod+Shift+l move right

# Split orientation
bindsym $mod+b splith
bindsym $mod+v splitv

# Layout styles
bindsym $mod+s layout stacking
bindsym $mod+w layout tabbed
bindsym $mod+e layout toggle split

# Toggle fullscreen / floating
bindsym $mod+f fullscreen toggle
bindsym $mod+Shift+space floating toggle

# Switch workspaces
bindsym $mod+1 workspace number 1
bindsym $mod+2 workspace number 2
bindsym $mod+3 workspace number 3

# Move window to workspace
bindsym $mod+Shift+1 move container to workspace number 1
bindsym $mod+Shift+2 move container to workspace number 2

# Reload configuration
bindsym $mod+Shift+c reload

# Exit session
bindsym $mod+Shift+e exec swaynag -t warning -m 'Exit Sway?' -B 'Yes' 'swaymsg exit'
```

---

## 4. Window Rules & Floating Windows

```ini
# Floating specific windows
for_window [app_id="pavucontrol"] floating enable
for_window [class="Pavucontrol"] floating enable
for_window [title="Picture-in-Picture"] floating enable, sticky enable

# Assign applications to workspaces
assign [app_id="firefox"] workspace number 2
assign [class="Code"] workspace number 3
```

---

## 5. Input Configuration (Touchpad & Keyboard in Sway)

```ini
input "type:touchpad" {
    dwt enabled
    tap enabled
    natural_scroll enabled
    middle_emulation enabled
}

input "type:keyboard" {
    xkb_layout "br"
    xkb_variant "abnt2"
}
```

---

## 6. Autostart Daemons

```ini
# Status Bar
exec_always waybar

# Notifications
exec swaync || mako

# Idle management (Swayidle)
exec swayidle -w \
    timeout 300 'swaylock -f -c 000000' \
    timeout 600 'swaymsg "output * power off"' \
    resume 'swaymsg "output * power on"' \
    before-sleep 'swaylock -f -c 000000'

# Polkit agent
exec /usr/lib/polkit-kde-authentication-agent-1
```

---

## 7. Controlling Sway/i3 from CLI

* **Send commands:** `swaymsg <command>` or `i3-msg <command>`
* **Reload config without restarting:** `swaymsg reload` or `i3-msg reload`
* **Query tree/windows:** `swaymsg -t get_tree`
