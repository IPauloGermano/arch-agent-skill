# Automation Hooks and Event Listeners (Arch Linux)

This guide covers event-driven scripting, Hyprland IPC hooks, and background automation on Arch Linux.

---

## 1. Hyprland Socket2 IPC Event Listener

Hyprland exposes an asynchronous UNIX domain socket (`.socket2.sock`) that broadcasts compositor events in real time (workspace changes, active window changes, monitor connections, etc.).

### Event Listener Script Pattern (`~/.config/hypr/scripts/event-listener.sh`):

```bash
#!/usr/bin/env bash

handle_event() {
  event="$1"
  case "$event" in
    workspace\>\>*)
      workspace_id="${event#*>>}"
      # Run custom action on workspace change
      ;;
    activewindow\>\>*)
      window_info="${event#*>>}"
      # Run custom action on window focus
      ;;
    monitoradded\>\>*)
      monitor_name="${event#*>>}"
      # Automatically configure new monitor or reload waybar
      hyprctl reload
      killall waybar && waybar &
      ;;
    monitorremoved\>\>*)
      hyprctl reload
      ;;
  esac
}

# Connect to the Hyprland event socket
socat -U - UNIX-CONNECT:"$XDG_RUNTIME_DIR/hypr/$HYPRLAND_INSTANCE_SIGNATURE/.socket2.sock" | while read -r line; do
  handle_event "$line"
done
```

Make executable:
```bash
chmod +x ~/.config/hypr/scripts/event-listener.sh
```

Autostart in `autostart.conf`:
```ini
exec-once = ~/.config/hypr/scripts/event-listener.sh
```

---

## 2. Systemd User Services and Timers

For background daemons that should restart automatically or run on a schedule:

### Example: Wallpaper Randomizer Timer
**Service file:** `~/.config/systemd/user/wallpaper-rotate.service`
```ini
[Unit]
Description=Rotate Wallpaper
After=graphical-session.target

[Service]
Type=oneshot
ExecStart=/usr/bin/bash -c "swww img $(find ~/Pictures/wallpapers -type f | shuf -n 1) --transition-type wipe"

[Install]
WantedBy=default.target
```

**Timer file:** `~/.config/systemd/user/wallpaper-rotate.timer`
```ini
[Unit]
Description=Rotate Wallpaper Every 30 Minutes

[Timer]
OnBootSec=5min
OnUnitActiveSec=30min

[Install]
WantedBy=timers.target
```

**Enable & Start:**
```bash
systemctl --user daemon-reload
systemctl --user enable --now wallpaper-rotate.timer
```

---

## 3. Power and Battery Automation

For low battery notifications without heavy background frameworks, use `udev` or a simple user cron/timer script with `notify-send`:

```bash
#!/usr/bin/env bash
capacity=$(cat /sys/class/power_supply/BAT*/capacity 2>/dev/null | head -n 1)
status=$(cat /sys/class/power_supply/BAT*/status 2>/dev/null | head -n 1)

if [[ "$status" == "Discharging" && "$capacity" -le 15 ]]; then
  notify-send -u critical -i battery-caution "Battery Critical" "Battery is at ${capacity}%. Connect charger."
fi
```

---

## 4. Power & Sleep Hooks (Hypridle)

Configure lock, suspend, and wakeup actions in `~/.config/hypr/hypridle.conf`:
```ini
general {
    lock_cmd = pidof hyprlock || hyprlock
    before_sleep_cmd = loginctl lock-session
    after_sleep_cmd = hyprctl dispatch dpms on
}
```
