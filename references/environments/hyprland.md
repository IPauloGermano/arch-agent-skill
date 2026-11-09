# Hyprland Configuration Guide (Arch Linux)

This guide covers the native configuration of the **Hyprland** Wayland compositor on Arch Linux.

---

## Configuration Architecture

On Arch Linux, Hyprland is configured via `hyprland.conf` using its native syntax. Many users adopt a modular configuration structure:

```
~/.config/hypr/
├── hyprland.conf        # Main configuration file (sources modular components)
├── monitors.conf        # Display modes, positions, scaling, and transform
├── keybindings.conf     # Application shortcuts, window management, media keys
├── windowrules.conf     # Floating rules, opacity, size, and workspace assignment
├── looknfeel.conf       # Gaps, borders, blur, rounding, and animations
├── autostart.conf       # Startup background daemons (exec-once)
├── hypridle.conf        # Idle timeouts, screen dimming, lock triggers
└── hyprlock.conf        # Lockscreen appearance and PAM authentication
```

In `~/.config/hypr/hyprland.conf`, include sub-configs with:
```ini
source = ~/.config/hypr/monitors.conf
source = ~/.config/hypr/keybindings.conf
source = ~/.config/hypr/windowrules.conf
source = ~/.config/hypr/looknfeel.conf
source = ~/.config/hypr/autostart.conf
```

---

## 1. Monitor Configuration (`monitors.conf`)

Syntax:
```ini
monitor = [name], [resolution@refresh_rate], [position], [scale], [options...]
```

### Examples:
```ini
# Auto-detect all connected monitors (safe fallback)
monitor = , preferred, auto, 1

# Specific monitor setups
monitor = eDP-1, 1920x1200@60, 0x0, 1
monitor = HDMI-A-1, 2560x1440@144, 1920x0, 1
monitor = DP-1, 3840x2160@60, 4480x0, 1.5

# Rotate/portrait monitor (transform: 1 = 90°, 2 = 180°, 3 = 270°)
monitor = DP-2, 1920x1080@60, 4480x0, 1, transform, 1

# Mirror display
monitor = DP-3, preferred, auto, 1, mirror, eDP-1

# Disable a monitor
monitor = Unknown-1, disable
```

*Query connected monitors:*
```bash
hyprctl monitors
hyprctl monitors all
```

---

## 2. Keybindings (`keybindings.conf`)

### Binding Modifiers & Flags
* `bind`: Standard keypress binding.
* `binde`: Repeatable when held down (volume, brightness).
* `bindl`: Locked (works even when the screen is locked).
* `bindel`: Both locked and repeatable.
* `bindm`: Mouse bind (move/resize windows).

### Common Keybindings Example:
```ini
$mainMod = SUPER
$terminal = kitty
$fileManager = nautilus
$menu = rofi -show drun -show-icons

# Applications
bind = $mainMod, RETURN, exec, $terminal
bind = $mainMod, E, exec, $fileManager
bind = $mainMod, SPACE, exec, $menu
bind = $mainMod, Q, killactive,
bind = $mainMod SHIFT, Q, exit,
bind = $mainMod, V, togglefloating,
bind = $mainMod, F, fullscreen, 0
bind = $mainMod, P, pseudo, # dwindle
bind = $mainMod, J, togglesplit, # dwindle

# Move focus with mainMod + arrow keys
bind = $mainMod, left, movefocus, l
bind = $mainMod, right, movefocus, r
bind = $mainMod, up, movefocus, u
bind = $mainMod, down, movefocus, d

# Switch workspaces with mainMod + [0-9]
bind = $mainMod, 1, workspace, 1
bind = $mainMod, 2, workspace, 2
bind = $mainMod, 3, workspace, 3
bind = $mainMod, 4, workspace, 4
bind = $mainMod, 5, workspace, 5

# Move active window to a workspace with mainMod + SHIFT + [0-9]
bind = $mainMod SHIFT, 1, movetoworkspace, 1
bind = $mainMod SHIFT, 2, movetoworkspace, 2
bind = $mainMod SHIFT, 3, movetoworkspace, 3
bind = $mainMod SHIFT, 4, movetoworkspace, 4
bind = $mainMod SHIFT, 5, movetoworkspace, 5

# Special workspace (scratchpad)
bind = $mainMod, S, togglespecialworkspace, magic
bind = $mainMod SHIFT, S, movetoworkspace, special:magic

# Scroll through existing workspaces
bind = $mainMod, mouse_down, workspace, e+1
bind = $mainMod, mouse_up, workspace, e-1

# Move/resize windows with mouse
bindm = $mainMod, mouse:272, movewindow
bindm = $mainMod, mouse:273, resizewindow

# Volume and Media Control (via WirePlumber / wpctl / playerctl)
bindel = , XF86AudioRaiseVolume, exec, wpctl set-volume -l 1.5 @DEFAULT_AUDIO_SINK@ 5%+
bindel = , XF86AudioLowerVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-
bindl  = , XF86AudioMute, exec, wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle
bindl  = , XF86AudioMicMute, exec, wpctl set-mute @DEFAULT_AUDIO_SOURCE@ toggle
bindl  = , XF86AudioPlay, exec, playerctl play-pause
bindl  = , XF86AudioNext, exec, playerctl next
bindl  = , XF86AudioPrev, exec, playerctl previous

# Brightness Control (via brightnessctl)
bindel = , XF86MonBrightnessUp, exec, brightnessctl s 10%+
bindel = , XF86MonBrightnessDown, exec, brightnessctl s 10%-
```

---

## 3. Window Rules v2 (`windowrules.conf`)

Hyprland uses `windowrulev2` for precise pattern-matching based on class, title, initialTitle, or tag:

```ini
# Floating dialogs and popups
windowrulev2 = float, class:^(pavucontrol|blueman-manager|nm-connection-editor|org.kde.polkit-kde-authentication-agent-1)$
windowrulev2 = float, title:^(Open File|Save File|Confirm to replace files|Choose wallpaper)$
windowrulev2 = size 800 500, class:^(pavucontrol)$
windowrulev2 = center, class:^(pavucontrol)$

# Pin picture-in-picture windows
windowrulev2 = float, title:^(Picture-in-Picture)$
windowrulev2 = pin, title:^(Picture-in-Picture)$
windowrulev2 = move 100%-w-20 100%-h-20, title:^(Picture-in-Picture)$

# Workspace assignment
windowrulev2 = workspace 2, class:^(firefox|chromium|google-chrome)$
windowrulev2 = workspace 3, class:^(code|vscodium)$
windowrulev2 = workspace 4, class:^(discord|vesktop|Slack|Spotify)$

# Opacity and blur overrides
windowrulev2 = opacity 0.90 0.85, class:^(kitty|Alacritty|foot|ghostty)$
windowrulev2 = idleinhibit focus, class:^(mpv|vlc)$
windowrulev2 = idleinhibit fullscreen, class:^(firefox|chromium)$

# Layer rules (for Waybar, Rofi, SwayNC, etc.)
layerrule = blur, waybar
layerrule = blur, rofi
layerrule = blur, swaync-control-center
layerrule = ignorezero, waybar
layerrule = ignorezero, rofi
```

*Query window classes and titles:*
```bash
hyprctl clients
```

---

## 4. Look & Feel (`looknfeel.conf`)

```ini
general {
    gaps_in = 5
    gaps_out = 10
    border_size = 2
    col.active_border = rgba(89b4faee) rgba(cba6f7ee) 45deg
    col.inactive_border = rgba(585b70aa)
    layout = dwindle
    resize_on_border = true
    allow_tearing = false
}

decoration {
    rounding = 10
    active_opacity = 1.0
    inactive_opacity = 0.92

    blur {
        enabled = true
        size = 6
        passes = 3
        new_optimizations = true
        xray = false
        vibrancy = 0.1696
    }

    shadow {
        enabled = true
        range = 15
        render_power = 3
        color = rgba(1a1a1aee)
    }
}

animations {
    enabled = true
    bezier = easeOutQuint, 0.23, 1, 0.32, 1
    bezier = easeInOutCubic, 0.65, 0, 0.35, 1
    
    animation = windows, 1, 4, easeOutQuint
    animation = windowsIn, 1, 4, easeOutQuint, popin 80%
    animation = windowsOut, 1, 4, easeOutQuint, popin 80%
    animation = border, 1, 6, default
    animation = fade, 1, 4, default
    animation = workspaces, 1, 4, easeOutQuint, slide
}

dwindle {
    pseudotile = true
    preserve_split = true
}

master {
    new_status = master
}

misc {
    force_default_wallpaper = 0
    disable_hyprland_logo = true
    disable_splash_rendering = true
    vfr = true
}
```

---

## 5. Startup Daemons (`autostart.conf`)

```ini
# Polkit Authentication Agent
exec-once = /usr/lib/polkit-kde-authentication-agent-1

# Status bar and wallpaper daemon
exec-once = waybar
exec-once = swww-daemon || hyprpaper

# Notification daemon
exec-once = swaync || dunst || mako

# Idle management
exec-once = hypridle

# Audio and Bluetooth
exec-once = pipewire
exec-once = wireplumber
exec-once = blueman-applet
exec-once = nm-applet --indicator

# Clipboard manager (cliphist)
exec-once = wl-paste --type text --watch cliphist store
exec-once = wl-paste --type image --watch cliphist store

# Desktop portal reset script (improves screen-sharing startup)
exec-once = ~/.config/hypr/scripts/xdg-portal-reset.sh
```

---

## 6. Reloading and Validating

After saving changes:
```bash
# Force reload configuration
hyprctl reload

# Check for syntax errors
hyprctl configerrors
```

---

## 7. Night Light & Blue Light Filtering (`hyprsunset`)

On modern Arch Linux with Hyprland, blue light filtering is handled via `hyprsunset`:

```bash
# Install hyprsunset
sudo pacman -S hyprsunset

# Run manually or test color temperature (in Kelvin)
hyprsunset -t 4000
```

### Configuration (`~/.config/hypr/hyprsunset.conf`):
```ini
# Default daytime temperature (off/neutral)
temperature = 6500

# Night profile (example custom setting)
profile {
    name = night
    temperature = 4200
}
```

Autostart via `autostart.conf`:
```ini
exec-once = hyprsunset
```

---

## 8. Desktop Portal & Screen Sharing (`xdph.conf`)

`xdg-desktop-portal-hyprland` configures screencopy, sharing modes, and picker behavior in `~/.config/hypr/xdph.conf`:

```ini
screencopy {
    max_fps = 60
    allow_token_by_default = true
    custom_picker_binary = default
}
```

---

## 9. Session Management & Systemd Integration (`uwsm`)

Universal Wayland Session Manager (`uwsm`) wraps compositor and desktop application launches inside managed systemd cgroups, preventing runaway background processes and ensuring clean environment variable inheritance.

### Launching Apps via UWSM:
In keybindings or launchers, prefix GUI apps with `uwsm app --`:
```ini
bind = $mainMod, Return, exec, uwsm app -- foot
bind = $mainMod, E, exec, uwsm app -- nautilus
bind = $mainMod, B, exec, uwsm app -- firefox
```

