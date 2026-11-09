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

---

## 5. Media Compression & Transcoding (`ffmpeg`)

Shrink large screen recordings or clips before uploading or sending to chat applications:

```bash
# Compress recording for chat/Discord sharing (H.264 CRF 28 + AAC audio)
ffmpeg -i input.mp4 -vcodec libx264 -crf 28 -preset fast -acodec aac -b:a 128k output_compact.mp4

# Downscale high-resolution capture (e.g. 4K/1440p down to 1080p)
ffmpeg -i input.mp4 -vf "scale=-2:1080" -c:v libx264 -crf 24 output_1080p.mp4

# Convert recording to optimized animated GIF
ffmpeg -i input.mp4 -vf "fps=15,scale=720:-1:flags=lanczos,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" recording.gif
```

---

## 6. Local & Mesh File Sharing (`LocalSend` & `Tailscale`)

Seamlessly send captured screenshots, screen recordings, or logs across devices:

### Option A: LocalSend (Local Wi-Fi Network Transfer)
```bash
# Install GUI or CLI
sudo pacman -S localsend
# or AUR CLI:
yay -S localsend-bin

# Send file via CLI to a nearby machine
localsend send ~/Pictures/Screenshots/capture.png
```

### Option B: Tailscale Taildrop (Encrypted Peer-to-Peer Transfer)
```bash
# Send screenshot to another machine on your Tailnet
tailscale file cp ~/Pictures/Screenshots/capture.png <machine_name>:

# Check or receive incoming files on local machine
tailscale file get .
```

