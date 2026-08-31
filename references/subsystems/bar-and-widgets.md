# Status Bar, Launchers, and Widgets (Waybar, Rofi, SwayNC, Hyprlock)

This guide covers the standard UI components and widgets commonly used on Arch Linux with Hyprland.

---

## 1. Status Bar: Waybar

Waybar is configured in `~/.config/waybar/`:
* `config.jsonc` (or `config`): Bar layout, modules, and behavior.
* `style.css`: Visual styling, borders, colors, and padding using GTK CSS.

### Basic Structure (`config.jsonc`):
```jsonc
{
  "layer": "top",
  "position": "top",
  "height": 34,
  "spacing": 4,
  "modules-left": [
    "hyprland/workspaces",
    "hyprland/window"
  ],
  "modules-center": [
    "clock"
  ],
  "modules-right": [
    "pulseaudio",
    "network",
    "cpu",
    "memory",
    "battery",
    "tray",
    "custom/power"
  ],
  "hyprland/workspaces": {
    "disable-scroll": true,
    "all-outputs": true,
    "format": "{name}"
  },
  "clock": {
    "format": " {:%H:%M}",
    "format-alt": " {:%Y-%m-%d}",
    "tooltip-format": "<tt><small>{calendar}</small></tt>"
  },
  "pulseaudio": {
    "format": "{icon} {volume}%",
    "format-muted": " Muted",
    "format-icons": {
      "default": ["", "", ""]
    },
    "on-click": "pavucontrol"
  },
  "network": {
    "format-wifi": " {essid}",
    "format-ethernet": "󰈀 {ipaddr}",
    "format-disconnected": "⚠ Disconnected",
    "on-click": "nm-connection-editor"
  },
  "battery": {
    "states": {
      "warning": 30,
      "critical": 15
    },
    "format": "{icon} {capacity}%",
    "format-charging": "󰂄 {capacity}%",
    "format-icons": ["󰂎", "󰁺", "󰁻", "󰁼", "󰁽", "󰁾", "󰁿", "󰂀", "󰂁", "󰂂", "󰁹"]
  },
  "custom/power": {
    "format": "",
    "tooltip": false,
    "on-click": "wlogout || rofi -show power-menu"
  }
}
```

### Styling (`style.css`):
```css
* {
  border: none;
  border-radius: 0;
  font-family: "JetBrainsMono Nerd Font", Roboto, sans-serif;
  font-size: 13px;
  min-height: 0;
}

window#waybar {
  background-color: rgba(26, 27, 38, 0.85);
  color: #c0caf5;
  border-bottom: 2px solid rgba(122, 162, 247, 0.5);
}

#workspaces button {
  padding: 0 8px;
  color: #565f89;
  border-radius: 6px;
  margin: 4px 2px;
}

#workspaces button.active {
  color: #7aa2f7;
  background-color: rgba(122, 162, 247, 0.15);
  font-weight: bold;
}

#workspaces button.urgent {
  color: #f7768e;
}

#clock, #battery, #cpu, #memory, #network, #pulseaudio, #tray {
  padding: 0 10px;
  margin: 4px 2px;
  border-radius: 6px;
  background-color: rgba(41, 46, 66, 0.7);
}

#battery.critical:not(.charging) {
  background-color: #f7768e;
  color: #1a1b26;
  animation-name: blink;
  animation-duration: 0.5s;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
  animation-direction: alternate;
}
```

### Reloading Waybar:
```bash
killall waybar && waybar &
# or if managed via systemd user unit:
systemctl --user restart waybar
```

---

## 2. Application Launcher: Rofi (Wayland)

Rofi with Wayland support (`rofi-wayland`) is configured at `~/.config/rofi/config.rasi`:

```rasi
configuration {
  modi: "drun,run,window";
  icon-theme: "Papirus";
  show-icons: true;
  terminal: "kitty";
  drun-display-format: "{icon} {name}";
  location: 0;
  disable-history: false;
  hide-scrollbar: true;
  display-drun: "   Apps ";
  display-run: "   Run ";
  display-window: " 󰕰  Window ";
}

@theme "~/.config/rofi/theme.rasi"
```

---

## 3. Notification Center: SwayNC

SwayNC (`swaynotificationcenter`) provides an expandable notification center and widget tray.

* Configuration: `~/.config/swaync/config.json`
* Style: `~/.config/swaync/style.css`

Key commands:
```bash
swaync-client -t          # Toggle notification center panel
swaync-client -d          # Toggle Do Not Disturb
swaync-client -C          # Clear all notifications
swaync-client -R          # Reload configuration and CSS
```

Bind in `keybindings.conf`:
```ini
bind = $mainMod, N, exec, swaync-client -t -sw
```

---

## 4. Idle & Lock: Hypridle and Hyprlock

### Hypridle (`~/.config/hypr/hypridle.conf`)
Manages screen dimming, lock timeouts, and DPMS sleep:
```ini
general {
    lock_cmd = pidof hyprlock || hyprlock       # dbus/systemd lock command
    before_sleep_cmd = loginctl lock-session    # lock before suspend
    after_sleep_cmd = hyprctl dispatch dpms on  # wake monitor after resume
}

# Dim screen after 2.5 minutes
listener {
    timeout = 150
    on-timeout = brightnessctl -s set 10
    on-resume = brightnessctl -r
}

# Lock screen after 5 minutes
listener {
    timeout = 300
    on-timeout = loginctl lock-session
}

# Turn off display after 5.5 minutes
listener {
    timeout = 330
    on-timeout = hyprctl dispatch dpms off
    on-resume = hyprctl dispatch dpms on
}

# Suspend system after 30 minutes
listener {
    timeout = 1800
    on-timeout = systemctl suspend
}
```

### Hyprlock (`~/.config/hypr/hyprlock.conf`)
```ini
background {
    monitor =
    path = /home/user/Pictures/wallpapers/lockscreen.png
    blur_passes = 2
    blur_size = 7
}

input-field {
    monitor =
    size = 250, 50
    outline_thickness = 3
    dots_size = 0.2
    dots_spacing = 0.2
    dots_center = true
    fade_on_empty = false
    placeholder_text = <span foreground="##cdd6f4">Enter Password...</span>
    hide_input = false
    position = 0, -20
    halign = center
    valign = center
}
```
