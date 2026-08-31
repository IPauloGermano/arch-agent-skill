# Package Management and System Maintenance (Arch Linux)

This guide covers package management (`pacman`, `yay`, `paru`), system updates, mirror optimization, GPU configuration, and system diagnostics on Arch Linux.

---

## 1. Package Management (`pacman` & AUR)

### Official Repositories (`pacman`):
```bash
# Update package databases and upgrade entire system
sudo pacman -Syu

# Install packages
sudo pacman -S <package_name>

# Remove package and its unneeded dependencies
sudo pacman -Rns <package_name>

# Search for packages in repositories
pacman -Ss <keyword>

# Search locally installed packages
pacman -Qs <keyword>

# Show package details
pacman -Qi <package_name>

# Clean package cache (keep latest 2 versions with paccache)
sudo pacman -S pacman-contrib
paccache -r

# Remove orphan packages (dependencies no longer required by any software)
sudo pacman -Rns $(pacman -Qtdq)
```

### AUR Helpers (`yay` / `paru`):
> [!IMPORTANT]
> **NEVER run `yay` or `paru` with `sudo`!** AUR helpers build packages in user space and will prompt for `sudo` only when invoking `pacman` to install the compiled package.

```bash
# Upgrade system including AUR packages
yay -Syu
# or
paru -Syu

# Install AUR package
yay -S <aur_package_name>

# Search AUR and official repos
yay -Ss <keyword>
```

---

## 2. Mirror Optimization (`reflector`)

Keep your download speeds optimal by updating your mirrorlist with fast, up-to-date mirrors:

```bash
sudo pacman -S reflector

# Rank fastest HTTPS mirrors
sudo reflector --latest 10 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

---

## 3. GPU Drivers & Wayland Compatibility

### NVIDIA Configuration for Hyprland
On NVIDIA hardware, ensure kernel modesetting and Wayland environment variables are active:

1. **Kernel parameter in bootloader** (GRUB, systemd-boot, or Limine):
   `nvidia_drm.modeset=1`

2. **Hyprland Environment Variables (`~/.config/hypr/hyprland.conf`):**
   ```ini
   env = LIBVA_DRIVER_NAME,nvidia
   env = GBM_BACKEND,nvidia-drm
   env = __GLX_VENDOR_LIBRARY_NAME,nvidia
   env = NVD_BACKEND,direct
   env = ELECTRON_OZONE_PLATFORM_HINT,auto
   ```

### AMD & Intel Hardware Acceleration
```bash
# AMD:
sudo pacman -S mesa vulkan-radeon libva-mesa-driver

# Intel:
sudo pacman -S mesa vulkan-intel intel-media-driver
```

---

## 4. System Diagnostics and Troubleshooting

### Failed Services:
```bash
# Check failed system-wide services
systemctl --failed

# Check failed user-space services (Waybar, PipeWire, SwayNC, etc.)
systemctl --user --failed
```

### System Logs (`journalctl`):
```bash
# Errors from the current boot
journalctl -p 3 -xb

# Logs for a specific systemd unit
journalctl -u <service_name> -b

# User service logs
journalctl --user -u pipewire -b
```

### Audio (PipeWire) Health Check:
```bash
systemctl --user status pipewire wireplumber pipewire-pulse
wpctl status
```

### Bluetooth Service:
```bash
sudo systemctl enable --now bluetooth
bluetoothctl power on
```
