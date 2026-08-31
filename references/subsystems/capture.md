# Screen Capture, Recording, and Sharing (Arch Linux)

This guide covers Wayland screen capture, video recording, OCR text extraction, and desktop sharing on Arch Linux with Hyprland.

---

## 1. Screenshots (`grim`, `slurp`, `grimblast`, `swappy`)

### Required Packages:
```bash
sudo pacman -S grim slurp wl-clipboard swappy
# Optional AUR helper script:
yay -S grimblast-git
```

### CLI Commands:
```bash
# Fullscreen to clipboard and file
grim ~/Pictures/Screenshots/$(date +'%Y-%m-%d_%H-%M-%S').png
grim - | wl-copy

# Region selection
grim -g "$(slurp)" ~/Pictures/Screenshots/$(date +'%Y-%m-%d_%H-%M-%S').png
grim -g "$(slurp)" - | wl-copy

# With Grimblast (Interactive / Clean):
grimblast copy area
grimblast save active ~/Pictures/Screenshots/
grimblast copysave output

# With Swappy (Annotation Editor):
grim -g "$(slurp)" - | swappy -f -
```

### Keybinding Definitions (`keybindings.conf`):
```ini
# Screenshot full screen -> clipboard
bind = , Print, exec, grimblast copy output

# Screenshot selected region -> clipboard
bind = SHIFT, Print, exec, grimblast copy area

# Screenshot active window -> clipboard
bind = CTRL, Print, exec, grimblast copy active

# Screenshot region and edit with Swappy
bind = SUPER SHIFT, S, exec, grim -g "$(slurp)" - | swappy -f -
```

---

## 2. Screen Recording (`wf-recorder`, `gpu-screen-recorder`)

### Option A: `wf-recorder`
```bash
# Record entire screen (default display)
wf-recorder -f ~/Videos/recordings/$(date +'%Y-%m-%d_%H-%M-%S').mp4

# Record selected region
wf-recorder -g "$(slurp)" -f ~/Videos/recordings/region.mp4

# Record with microphone / desktop audio (PulseAudio/PipeWire)
wf-recorder --audio -f ~/Videos/recordings/with_audio.mp4

# Stop active recording:
killall -s SIGINT wf-recorder
```

### Option B: `gpu-screen-recorder` (High-Performance Hardware NVENC/VAAPI)
```bash
sudo pacman -S gpu-screen-recorder
# Fast recording without CPU overhead:
gpu-screen-recorder -w screen -f 60 -a default_output -o ~/Videos/gameplay.mp4
```

---

## 3. Text Capture / OCR (Extract Text from Screen)

Extract selectable text directly from any part of the screen into the clipboard using `tesseract`:

### Setup:
```bash
sudo pacman -S tesseract tesseract-data-eng tesseract-data-por
```

### OCR One-liner Script:
```bash
grim -g "$(slurp)" -t png - | tesseract - - -l eng+por | wl-copy && notify-send "OCR" "Text copied to clipboard"
```

### Bind in `keybindings.conf`:
```ini
bind = SUPER SHIFT, T, exec, grim -g "$(slurp)" -t png - | tesseract - - | wl-copy && notify-send "OCR" "Text copied to clipboard"
```

---

## 4. Screen Sharing & WebRTC Portals (Discord, OBS, Browser)

Wayland screen sharing relies on `pipewire`, `wireplumber`, and `xdg-desktop-portal-hyprland`.

### Required Packages:
```bash
sudo pacman -S pipewire wireplumber xdg-desktop-portal-hyprland xdg-desktop-portal-gtk
```

### Portal Priority Setup (`~/.config/hypr/xdph.conf` or `~/.config/xdg-desktop-portal/portals.conf`):
```ini
[preferred]
default = hyprland;gtk
```

### Portal Troubleshooting Reset Script (`~/.config/hypr/scripts/xdg-portal-reset.sh`):
```bash
#!/usr/bin/env bash
sleep 1
killall -e xdg-desktop-portal-hyprland
killall -e xdg-desktop-portal-gtk
killall -e xdg-desktop-portal
/usr/lib/xdg-desktop-portal-hyprland &
sleep 2
/usr/lib/xdg-desktop-portal &
```
