# Arch Linux Dual Boot Setup (Windows 11 + Hyprland)

> Personal reference for my Lenovo IdeaPad 5 setup.
> Written to be my eyes and ears during install — every failure mode documented.

---

## Table of Contents

1. [Pre-Install: Windows Prep](#1-pre-install-windows-prep)
2. [Flash the USB](#2-flash-the-usb)
3. [BIOS Setup](#3-bios-setup)
4. [Boot into Live Environment](#4-boot-into-live-environment)
5. [Internet Setup (Ethernet-to-USB)](#5-internet-setup-ethernet-to-usb)
6. [Partitioning](#6-partitioning)
7. [Running archinstall](#7-running-archinstall)
8. [Post-Install: GRUB & First Boot](#8-post-install-grub--first-boot)
9. [First Boot: Hyprland Setup](#9-first-boot-hyprland-setup)
10. [Accessing Windows Files from Arch](#10-accessing-windows-files-from-arch)
11. [Things That Can Go Wrong](#11-things-that-can-go-wrong)

---

## 1. Pre-Install: Windows Prep

### Disable Hibernation (CRITICAL)
Run in admin PowerShell:
```powershell
powercfg /h off
```
**Why:** Fast Startup uses hibernation. If Windows is hibernated and Arch touches the NTFS partition, it will corrupt it. Disabling hibernation kills Fast Startup too.

**Verify:** `C:\hiberfil.sys` should no longer exist after running this.

### Disable Fast Startup
Control Panel → Power Options → "Choose what the power buttons do" → uncheck **Turn on fast startup**.

> If this option doesn't appear, hibernation is already off and Fast Startup is already disabled. You're fine.

### Free Up Space
You need at least 40 GB unallocated, ideally 80 GB. Things that eat space silently:
- `uv` cache: `uv cache clean`
- Gradle cache: `rm -rf ~/.gradle/caches`
- npm cache: `npm cache clean --force`
- WSL vhd: `C:\Users\<user>\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu*\LocalState\ext4.vhdx`
- Downloads folder (installers you've already used)

### Try Shrinking in Disk Management
`Win+R` → `diskmgmt.msc` → right-click C: → Shrink Volume

**If it only offers 2 GB or less:** Windows has unmovable files blocking the shrink (pagefile, MFT, restore points). Skip this — partition from the Arch live environment instead using `cfdisk`. It bypasses Windows restrictions entirely.

---

## 2. Flash the USB

Use **Rufus**:
- Partition scheme: **GPT**
- Target system: **UEFI (non-CSM)**
- File system: FAT32
- When prompted: select **DD Image mode** (not ISO mode)

**Why DD mode:** ISO mode can cause boot failures on some UEFI systems. DD writes the image as-is.

**What can go wrong:**
- USB not recognized in BIOS → try a different USB port (use USB 2.0 if available, some UEFI firmwares have issues with USB 3.0 during boot)
- Rufus says "ISOHybrid" → always pick DD
- Flashed wrong drive → double-check device before clicking Start

---

## 3. BIOS Setup

Power on → spam **F2** to enter BIOS (Lenovo IdeaPad 5)

**Changes to make:**
1. Security → Secure Boot → **Disabled**
2. Boot → USB Boot → **Enabled**
3. F10 to save and exit

**What can go wrong:**
- Secure Boot not disabled → USB boots but drops to a black screen or "Verification failed" error. Go back into BIOS and disable it.
- BIOS key varies: if F2 doesn't work, try Fn+F2, or the small reset pinhole button on the side of the laptop
- Some IdeaPad 5 models have a "Novo button" (small button near power port) — press it with a pin to get a boot menu without entering BIOS

---

## 4. Boot into Live Environment

Power on → spam **F12** → Boot Menu appears → select your USB (shows as `UEFI: [USB brand]`)

Select: **Arch Linux install medium (x86_64, UEFI)**

You should land at:
```
root@archiso ~ #
```

**What can go wrong:**
- F12 menu doesn't appear → you're too slow. Power off, try again, start spamming earlier
- USB boots to a black screen → Secure Boot is still on. Go back to BIOS
- "No bootable device" → USB wasn't flashed correctly, or BIOS doesn't see it. Reflash using DD mode
- Keyboard/trackpad not working in live env → they always work, this shouldn't happen. If it does, try a different USB port for the USB keyboard

---

## 5. Internet Setup (Ethernet-to-USB)

Plug in your ethernet-to-USB adapter **before** booting, or plug it in after you're at the shell.

**Check if it's recognized:**
```bash
ip link
```

You'll see interfaces listed. Look for one that's NOT `lo` (loopback) and NOT `wlan0`. It'll look like `enp0s20f0u1` or `eth0` or similar.

**Get an IP via DHCP:**
```bash
dhcpcd <interface-name>
```

**Test connection:**
```bash
ping -c 3 archlinux.org
```

**Sync time:**
```bash
timedatectl set-ntp true
```

**What can go wrong:**
- Interface not showing up → adapter not recognized. Run `lsusb` to see if the USB device is detected at all. Most common USB ethernet chips (AX88179, RTL8153) are supported in the Arch live kernel. If `lsusb` shows the device but `ip link` doesn't, run `dmesg | tail -20` to see the error.
- DHCP fails → try `ip link set <interface> up` first, then `dhcpcd`
- Ping fails but DHCP worked → DNS issue. Try `ping 8.8.8.8` (IP, not domain). If that works, add `nameserver 8.8.8.8` to `/etc/resolv.conf`
- Adapter keeps disconnecting → USB power issue. Try a powered USB hub or a different port

---

## 6. Partitioning

### Identify your disk
```bash
lsblk
```

Your disk will be `/dev/nvme0n1` (NVMe SSD) or `/dev/sda` (SATA). You'll see existing Windows partitions:

```
nvme0n1
├─nvme0n1p1   100M    ← Windows EFI (DO NOT TOUCH)
├─nvme0n1p2    16M    ← Windows MSR (DO NOT TOUCH)
├─nvme0n1p3   ~XGB   ← Windows C: (DO NOT TOUCH)
└─ free space         ← This is where you work
```

### Open cfdisk
```bash
cfdisk /dev/nvme0n1
```

### Create partitions in the free space:

1. Select free space → `[ New ]` → size: `8G` → Enter
   - Change type to: **Linux swap**

2. Select remaining free space → `[ New ]` → accept full size
   - Type stays: **Linux filesystem**

3. `[ Write ]` → type `yes` → `[ Quit ]`

### Format
```bash
lsblk   # note your new partition names, e.g. nvme0n1p4 and nvme0n1p5

mkfs.ext4 /dev/nvme0n1p5   # root partition (larger one)
mkswap /dev/nvme0n1p4       # swap partition
swapon /dev/nvme0n1p4
```

**What can go wrong:**
- Accidentally selected a Windows partition in cfdisk → **do not write**. Press `[ Quit ]` without writing and start over
- "Partition table not found" → run `gdisk /dev/nvme0n1` and check. Your disk should be GPT. If it says MBR, your Windows install is legacy — unusual on a modern IdeaPad but possible
- `mkfs.ext4` fails with "device busy" → something mounted the partition automatically. Run `umount /dev/nvme0n1p5` then retry
- Free space not showing in cfdisk → it may show as "unusable" if Windows left it misaligned. Use `parted` instead: `parted /dev/nvme0n1` → `mkpart primary ext4 XGB 100%`
- Not enough free space to partition → you need to free more from Windows first, or shrink the Windows partition with `ntfsresize`:
  ```bash
  ntfsresize --size 300G /dev/nvme0n1p3   # shrink Windows to 300GB
  ```
  Then update the partition table in cfdisk to match

---

## 7. Running archinstall

```bash
archinstall
```

### Key selections:

| Option | Value |
|---|---|
| Mirrors | your country |
| Locales | en_US.UTF-8 |
| Disk configuration | **Pre-mounted configuration** |
| Mount `/` | your root partition (e.g. nvme0n1p5) |
| Mount `/boot/efi` | **existing Windows EFI** (nvme0n1p1) — **do NOT format** |
| Bootloader | **Grub** |
| Swap | No (done manually) |
| Hostname | e.g. `tarun-arch` |
| Root password | set one |
| User | create user, enable sudo |
| Profile | Desktop → **Hyprland** |
| Audio | **Pipewire** |
| Kernels | `linux` |
| Additional packages | `firefox git base-devel networkmanager` |
| Network | **NetworkManager** |
| Timezone | Asia/Kolkata |
| NTP | Yes |

**What can go wrong:**
- Can't find EFI partition → run `lsblk -f` and look for a partition with `vfat` filesystem and ~100MB size. That's your EFI
- archinstall crashes mid-way → run it again, it's safe to restart
- "not enough space on EFI partition" → the Windows EFI partition (100MB) is shared. It usually has plenty of room for GRUB (~5MB). If not, you can resize it but that's advanced — unlikely to happen
- Profile → Hyprland not showing → make sure you selected "Desktop" first, then Hyprland appears as a sub-option
- Package download fails → your internet dropped. Re-run `dhcpcd <interface>` and restart archinstall

---

## 8. Post-Install: GRUB & First Boot

After archinstall finishes, it asks if you want to chroot into the new system. **Say yes.**

Inside chroot:
```bash
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

`grub-mkconfig` will scan all partitions and automatically find Windows Boot Manager. You'll see output like:
```
Found Windows Boot Manager on /dev/nvme0n1p1
```

Then:
```bash
exit
reboot
```

Remove the USB when prompted.

**On reboot you'll see:**
```
Arch Linux
Arch Linux (fallback initramfs)
Windows Boot Manager
```

Arrow keys to select, Enter to boot. Defaults to Arch after timeout.

**What can go wrong:**
- GRUB not showing, boots straight to Windows → Windows Boot Manager took over. Fix: boot from USB again → `arch-chroot /mnt` → re-run grub-install and grub-mkconfig. Or enter BIOS and set GRUB as the first boot entry
- GRUB shows but Windows option missing → run `sudo grub-mkconfig -o /boot/grub/grub.cfg` from within Arch. It'll find Windows
- GRUB shows "error: unknown filesystem" → EFI partition may not be mounted correctly. Boot live USB, mount everything and chroot:
  ```bash
  mount /dev/nvme0n1p5 /mnt
  mount /dev/nvme0n1p1 /mnt/boot/efi
  arch-chroot /mnt
  grub-install ...
  ```
- System boots to black screen after GRUB → Hyprland failed to start. TTY should still work (Ctrl+Alt+F2). Log in and check `journalctl -xe`
- "Failed to mount /boot/efi" on boot → add the EFI partition to `/etc/fstab`. Get its UUID with `blkid` and add:
  ```
  UUID=XXXX  /boot/efi  vfat  defaults  0 2
  ```

---

## 9. First Boot: Hyprland Setup

Log in with your user. Hyprland starts but it's bare — that's normal.

**Default keybindings:**
- `Super + Q` → terminal (Kitty)
- `Super + R` → app launcher (if wofi installed)
- `Super + M` → exit Hyprland
- `Super + arrow keys` → move focus

**First commands:**
```bash
# Connect ethernet
nmcli device connect <interface>

# Update system
sudo pacman -Syu

# Install yay (AUR helper)
cd /tmp
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

# Essential Hyprland ecosystem
sudo pacman -S waybar wofi kitty thunar

# Fonts (important, many things break without them)
sudo pacman -S ttf-jetbrains-mono noto-fonts noto-fonts-emoji
```

**Hyprland config lives at:**
```
~/.config/hypr/hyprland.conf
```

Edit this to set keybindings, monitors, gaps, animations, autostart apps etc.

**What can go wrong:**
- Black screen after login → Hyprland crashed. `Ctrl+Alt+F2` to get a TTY. Check `cat ~/.local/share/hyprland/hyprland.log`
- No cursor visible → add `exec-once = hyprctl setcursor <theme> 24` to hyprland.conf
- Display not detected correctly → add monitor config to hyprland.conf: `monitor=,preferred,auto,1`
- Waybar not starting → check `~/.config/waybar/config` exists. Copy default: `cp /etc/xdg/waybar/ ~/.config/waybar/ -r`
- No audio → run `systemctl --user enable --now pipewire pipewire-pulse wireplumber`
- Ethernet not connecting on boot → `sudo systemctl enable NetworkManager`
- Pacman keyring errors → `sudo pacman-key --init && sudo pacman-key --populate archlinux`

---

## 10. Accessing Windows Files from Arch

Your Windows C: drive is fully readable from Arch.

**Mount it:**
```bash
sudo mkdir /mnt/windows
sudo mount /dev/nvme0n1p3 /mnt/windows
```

Your files are now at `/mnt/windows/Users/ultim/`.

**Auto-mount on boot** — add to `/etc/fstab`:
```
/dev/nvme0n1p3  /mnt/windows  ntfs-3g  defaults,uid=1000,gid=1000  0 0
```

Install ntfs-3g first: `sudo pacman -S ntfs-3g`

**What can go wrong:**
- "The disk contains an unclean file system" → Windows wasn't shut down properly (fast startup). Boot into Windows, shut down fully (not restart), then try again
- Read-only mount → same cause as above, or the partition is hibernated. Full Windows shutdown fixes it
- Permission denied on files → add `uid=1000,gid=1000` to mount options

---

## 11. Things That Can Go Wrong (Master List)

### Before Install
| Problem | Fix |
|---|---|
| Disk Management only offers 2 MB shrink | Partition from Arch live environment using cfdisk instead |
| Fast Startup still on | Run `powercfg /h off` in admin PowerShell — this kills it |
| USB not booting | Reflash with Rufus in DD mode; try different USB port |

### In Live Environment
| Problem | Fix |
|---|---|
| No internet | `ip link` → `dhcpcd <interface>` → `ping 8.8.8.8` |
| Ethernet adapter not detected | `lsusb` to verify it's seen; `dmesg | tail` for errors |
| Wrong partition deleted | Don't write in cfdisk — quit and start over |
| archinstall crashes | Just run it again |
| EFI partition not found | `lsblk -f` — look for vfat ~100MB partition |

### After Install
| Problem | Fix |
|---|---|
| Boots straight to Windows, no GRUB | Boot live USB → chroot → re-run grub-install |
| GRUB shows, Windows missing | `sudo grub-mkconfig -o /boot/grub/grub.cfg` |
| Black screen after GRUB | TTY with Ctrl+Alt+F2; check Hyprland logs |
| No audio | `systemctl --user enable --now pipewire pipewire-pulse wireplumber` |
| No network on boot | `sudo systemctl enable NetworkManager` |
| Windows files read-only | Boot Windows → full shutdown (not sleep/restart) → remount |
| pacman keyring errors | `sudo pacman-key --init && sudo pacman-key --populate archlinux` |
| yay won't build | Install `base-devel` first: `sudo pacman -S base-devel` |

### Nuclear Option
If something is catastrophically broken and Arch won't boot:
1. Boot from Arch USB
2. Mount your partitions:
   ```bash
   mount /dev/nvme0n1p5 /mnt
   mount /dev/nvme0n1p1 /mnt/boot/efi
   arch-chroot /mnt
   ```
3. Fix whatever's broken from inside chroot
4. Windows is always accessible — it's untouched on its own partition

---

## Hardware Reference (Lenovo IdeaPad 5)

| Key | Function |
|---|---|
| F2 | Enter BIOS |
| F12 | Boot menu |
| Novo button | Emergency boot menu (pinhole near power port) |

- Disk: NVMe SSD → `/dev/nvme0n1`
- Internet during install: ethernet-to-USB adapter
- Target DE: Hyprland + Waybar + Wofi + Kitty
- Audio: Pipewire
- Bootloader: GRUB (dual boots with Windows 11)
