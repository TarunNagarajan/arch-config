<h1 align="center">Retrilz's Dotfiles</h1>

<table align="center">
   <tr>
      <td colspan="3" align="center">
         <img src="https://share.rzx.ovh/go/rdots-general?v=2" width="fit"><br>
         <sub><b>General</b></sub>
      </td>
   </tr>
   <tr>
      <td align="center">
         <img src="https://share.rzx.ovh/go/rdots-hyprlock" width="fit"><br>
         <sub><b>Hyprlock</b></sub>
      </td>
      <td align="center">
         <img src="https://share.rzx.ovh/go/rdots-terminals?v=2" width="fit"><br>
         <sub><b>Terminals</b></sub>
      </td>
  </tr>
  <tr>
      <td colspan="3" align="center">
         <img src="https://share.rzx.ovh/go/rdots-gtk-qt?v=2" width="fit"><br>
         <sub><b>GTK & Qt</b></sub>
      </td>
   </tr>
</table>

<div align="center">
  <p>【 🇬🇧 English 】 <a href="./README-ru.md">【 🇷🇺 Русский 】</a></p>
   <img alt="Last README modification" src="https://img.shields.io/github/last-commit/retrilzzy/dotfiles?path=README.md&style=for-the-badge&logo=readdotcv&logoColor=ffff&label=Last%20README%20modification&labelColor=0D1117&color=0D1117">
</div>

# Navigation

- [Installation](#installation)
- [Detailed overview](#detailed-overview)
  - [Dependencies](#dependencies) - required packages.
  - [Hyprland](#hyprland) - dynamic tiling Wayland compositor.
    - [Keybinds](#keybinds) - all key combinations.
    - [Icons](#icons) - icon pack.
    - [Cursor](#cursor) - cursor theme.
    - [Fonts](#fonts) - font installation.
    - [Hypridle](#hypridle) - idle behavior.
    - [Hyprlock](#hyprlock) - lock screen.
  - [Waybar](#waybar) - Wayland bar.
  - [Vicinae](#vicinae) - focused launcher.
  - [Wlogout](#wlogout) - logout menu.
  - [Terminal](#terminal) - terminal settings.
  - [Nwg-look](#nwg-look) - GTK settings.
  - [Qt](#qt) - Qt settings.
  - [Swaync](#swaync) - notifications.
  - [Waypaper](#waypaper) - GUI for easy wallpaper management.
    - [Wallpapers](#wallpapers) - collection of wallpapers.
  - [Fastfetch](#fastfetch) - show off your Linux.

> [!WARNING]  
> My configs are not designed for universal use and are not 100% automated, so they may require manual adjustments. I do not guarantee the correct operation of the configs or software on your system.

# Installation

> [!NOTE]
> You must have a working Hyprland before installation.

> [!IMPORTANT]  
> The installation script has only been tested on **Arch Linux**.

0. System update:

   ```
   sudo pacman -Syu
   ```

1. Run the [installation script](./Scripts/install.sh):

   ```
   curl https://raw.githubusercontent.com/retrilzzy/dotfiles/refs/heads/main/Scripts/install.sh | bash
   ```

   - Installs the [necessary packages](#dependencies).
   - Clones this repository to `~/dotfiles`.
   - Creates a backup of your configs in `~/.config-backups/$date_time`.
   - Applies the new configs from this repository.

## After installation

Actions you probably want to do.

**General:**

- Add your wallpapers to `~/Pictures/Wallpapers`.
- Run `p10k configure` to configure the terminal theme.
- Remove unnecessary Zsh plugins for you in `~/.zshrc`:
  ```zsh
  plugins=(...)
  ```

**PC users:**

- Disable the `custom/backlight` Waybar module in `~/.config/waybar/config.jsonc`.

## Restoring config backup

```
~/dotfiles/Scripts/restore.sh
```

# Detailed overview

## Dependencies

Packages installed by [`install.sh`](./Scripts/install.sh).

<details>
<summary><b>System and interface</b></summary>

| Package    | Description                  |
| :--------- | :--------------------------- |
| `hyprlock` | Lock screen                  |
| `hypridle` | Idle management daemon       |
| `hyprshot` | Screenshot tool              |
| `kitty`    | Terminal emulator            |
| `nwg-look` | GTK theme configuration tool |
| `swaync`   | Notification center          |
| `vicinae`  | Native launcher              |
| `waybar`   | Wayland status bar           |
| `waypaper` | Wallpaper manager            |
| `wlogout`  | Logout menu                  |
| `swww`     | Wallpaper daemon             |
| `zsh`      | Command shell                |

</details>

<details>
<summary><b>Utilities and tools</b></summary>

| Package                  | Description                      |
| :----------------------- | :------------------------------- |
| `base-devel`             | Development tools for AUR builds |
| `brightnessctl`          | Brightness control               |
| `fastfetch`              | System information viewer        |
| `git`                    | Version control system           |
| `gpu-screen-recorder`    | GPU-accelerated screen recorder  |
| `grim`                   | Wayland screenshot tool          |
| `gnome-keyring`          | Stores encryption keys           |
| `imagemagick`            | Image manipulation tools         |
| `lsd`                    | `ls` alternative with icons      |
| `nautilus`               | File manager                     |
| `network-manager-applet` | Network management tray applet   |
| `pavucontrol`            | PulseAudio volume control        |
| `playerctl`              | Media player control             |
| `satty`                  | Screenshot tool                  |
| `trash-cli`              | Command-line trash utility       |
| `uwsm`                   | Wayland session manager          |
| `wl-clip-persist`        | Clipboard persistence            |
| `wl-clipboard`           | Wayland clipboard utilities      |
| `yay`                    | AUR helper                       |

</details>

<details>
<summary><b>Networking, audio and portals</b></summary>

| Package                       | Description                    |
| :---------------------------- | :----------------------------- |
| `bluez`                       | Bluetooth stack                |
| `blueman`                     | Bluetooth manager              |
| `networkmanager`              | Network connection manager     |
| `pipewire`                    | Audio and video server         |
| `pipewire-alsa`               | ALSA compatibility layer       |
| `pipewire-pulse`              | PulseAudio compatibility layer |
| `polkit-gnome`                | PolicyKit authentication agent |
| `xdg-desktop-portal`          | Desktop portal service         |
| `xdg-desktop-portal-gtk`      | GTK portal backend             |
| `xdg-desktop-portal-hyprland` | Hyprland portal backend        |
| `xdg-desktop-portal-wlr`      | wlroots portal backend         |
| `xdg-utils`                   | XDG integration utilities      |

</details>

<details>
<summary><b>Appearance and themes</b></summary>

| Package                   | Description                         |
| :------------------------ | :---------------------------------- |
| `adw-gtk-theme`           | Adwaita theme for GTK               |
| `darkly-bin`              | Qt5/Qt6 dark theme                  |
| `frameworkintegration`    | KDE workspace integration framework |
| `inter-font`              | Inter font family                   |
| `matugen-bin`             | Generate themes from wallpapers     |
| `noto-fonts`              | Noto font family                    |
| `noto-fonts-cjk`          | Noto fonts for CJK languages        |
| `noto-fonts-emoji`        | Noto emoji fonts                    |
| `noto-fonts-extra`        | Additional Noto fonts               |
| `papirus-icon-theme`      | Papirus icon theme                  |
| `qt5ct-kde`               | Qt5 theme configuration             |
| `qt6ct-kde`               | Qt6 theme configuration             |
| `ttf-jetbrains-mono-nerd` | JetBrains Mono with Nerd glyphs     |

</details>

## Hyprland

Dynamic tiling Wayland compositor.

- [[General](./Configs/.config/hypr/hyprland/general.conf)]
- [[Autostart](./Configs/.config/hypr/hyprland/autostart.conf)]
- [[Keybindings](./Configs/.config/hypr/hyprland/keybindings.conf)]
- [[Monitor configuration](./Configs/.config/hypr/hyprland/monitors.conf)]
- [[Window and workspace rules](./Configs/.config/hypr/hyprland/rules.conf)]

## Keybinds

<details>
   <summary>
      <b>Application launch</b>
   </summary>

| Keys                                               | Action                                |
| :------------------------------------------------- | :------------------------------------ |
| <kbd>Super</kbd> + <kbd>W</kbd>                    | Terminal (Kitty)                      |
| <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>W</kbd> | Terminal in floating mode             |
| <kbd>Super</kbd> + <kbd>R</kbd>                    | Application menu (Vicinae)            |
| <kbd>Super</kbd> + <kbd>E</kbd>                    | File manager (Nautilus)               |
| <kbd>Super</kbd> + <kbd>C</kbd>                    | Code editor (VSCodium\*)              |
| <kbd>Super</kbd> + <kbd>B</kbd>                    | Browser (Brave\*)                     |
| <kbd>Super</kbd> + <kbd>K</kbd>                    | Password manager (KeePassXC\*)        |
| <kbd>Super</kbd> + <kbd>V</kbd>                    | Clipboard (Vicinae Clipboard)         |
| <kbd>Super</kbd> + <kbd>N</kbd>                    | Notification center (SwayNC)          |
| <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>E</kbd> | Emoji menu (Vicinae Emoji)            |
| <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> | Wallpaper selector (Vicinae + Script) |

<b><i>\*VSCodium, Brave, KeePassXC</b> - are NOT installed automatically by the [installation script](./Scripts/install.sh).</i>

</details>

<details>
   <summary>
      <b>Window interaction</b>
   </summary>

| Keys                                                      | Action                              |
| :-------------------------------------------------------- | :---------------------------------- |
| <kbd>Super</kbd> + <kbd>Q</kbd>                           | Close active window                 |
| <kbd>F11</kbd>                                            | Fullscreen                          |
| <kbd>Super</kbd> + <kbd>A</kbd>                           | Maximize active window              |
| <kbd>Super</kbd> + <kbd>F</kbd>                           | Toggle window to "float" mode       |
| <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>A</kbd>        | Switch to pseudo-tiling mode        |
| <kbd>Super</kbd> + <kbd>S</kbd>                           | Pin window on top of all workspaces |
| <kbd>Super</kbd> + <kbd>D</kbd>                           | Toggle window split mode            |
| <kbd>Alt</kbd> + <kbd>Tab</kbd>                           | Switch to the next window           |
| <kbd>Super</kbd> + <kbd>Arrows</kbd>                      | Move focus between windows          |
| <kbd>Super</kbd> + <kbd>Control</kbd> + <kbd>Arrows</kbd> | Resize active window                |
| <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>Arrows</kbd>   | Move windows                        |
| <kbd>Super</kbd> + <kbd>LMB</kbd>                         | Move windows with the mouse         |
| <kbd>Super</kbd> + <kbd>RMB</kbd>                         | Resize windows with the mouse       |

</details>

<details>
   <summary>
      <b>Workspaces</b>
   </summary>

| Keys                                                   | Action                             |
| :----------------------------------------------------- | :--------------------------------- |
| <kbd>Super</kbd> + <kbd>[0-9]</kbd>                    | Switch between workspaces 1 to 10  |
| <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>[0-9]</kbd> | Move window to workspace 1 to 10   |
| <kbd>Super</kbd> + <kbd>Tab</kbd>                      | Switch to a special workspace      |
| <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>Tab</kbd>   | Move window to a special workspace |

</details>

<details>
   <summary>
      <b> Screen/power management</b>
   </summary>

| Keys                                             | Action                       |
| :----------------------------------------------- | :--------------------------- |
| <kbd>Super</kbd> + <kbd>L</kbd>                  | Lock screen                  |
| <kbd>Super</kbd> + <kbd>Alt</kbd> + <kbd>D</kbd> | Turn display on/off          |
| <kbd>Super</kbd> + <kbd>Alt</kbd> + <kbd>S</kbd> | Lock screen and put to sleep |

</details>

<details>
   <summary>
      <b>Screenshots</b>
   </summary>

| Keys                                               | Action                                                             |
| :------------------------------------------------- | :----------------------------------------------------------------- |
| <kbd>Print</kbd>                                   | Screenshot of the entire screen                                    |
| <kbd>Shift</kbd> + <kbd>Print</kbd>                | Screenshot of the selected area                                    |
| <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>S</kbd> | Satty (screenshot annotation tool)                                 |
| <kbd>Super</kbd> + <kbd>Print</kbd>                | Screenshot and auto upload to [Zipline](https://zipline.diced.sh/) |

</details>

<details>
   <summary>
      <b>Other</b>
   </summary>

| Keys                                               | Action                                                   |
| :------------------------------------------------- | :------------------------------------------------------- |
| <kbd>Super</kbd> + <kbd>Escape</kbd>               | Hide/show Waybar                                         |
| <kbd>Super</kbd> +<kbd>Alt</kbd> + <kbd>P</kbd>    | Random background + Generate theme (matugen)             |
| <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>R</kbd> | Start/stop recording a screen area (gpu-screen-recorder) |
| <kbd>Super</kbd> + <kbd>H</kbd>                    | Toggle windows and layers visibility on screen share     |
| <kbd>Super</kbd> + <kbd>M</kbd>                    | Mute/unmute microphone                                   |
| <kbd>Super</kbd> + <kbd>Z</kbd>                    | Zoom in                                                  |
| <kbd>Super</kbd> + <kbd>Mouse wheel</kbd>          | Zoom in/out                                              |
| <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>Z</kbd> | Reset zoom                                               |

</details>

## Icons

https://github.com/PapirusDevelopmentTeam/papirus-icon-theme

```
sudo pacman -S papirus-icon-theme
```

## Cursor

https://github.com/ful1e5/Bibata_Cursor

The [installation script](./Scripts/install.sh) installs the [`Bibata-Modern-Classic`](https://github.com/ful1e5/Bibata_Cursor/releases/tag/v2.0.7) cursor theme.

```
mkdir -p /tmp/bibata
curl -L https://github.com/ful1e5/Bibata_Cursor/releases/download/v2.0.7/Bibata-Modern-Classic.tar.xz -o /tmp/bibata/bibata.tar.xz
tar -xf /tmp/bibata/bibata.tar.xz -C /tmp/bibata
sudo cp -r /tmp/bibata/Bibata-Modern-Classic /usr/share/icons/
```

## Fonts

- [Noto](https://www.google.com/get/noto/) - all languages + emoji + special characters:

  ```
  sudo pacman -S noto-fonts noto-fonts-emoji noto-fonts-cjk noto-fonts-extra
  ```

- [JetBrains Mono Nerd](https://www.jetbrains.com/lp/mono/) for [Waybar](#waybar) and [Kitty](#terminal):

  ```
  sudo pacman -S ttf-jetbrains-mono-nerd
  ```

- [Inter](https://rsms.me/inter/) required font for [Hyprlock](#hyprlock):

  ```
  sudo pacman -S inter-font
  ```

## Hypridle

Idle behavior. [[config](./Configs/.config/hypr/hypridle.conf)]

https://wiki.hypr.land/Hypr-Ecosystem/hypridle |
https://github.com/hyprwm/hypridle

```
sudo pacman -S hypridle
```

| Action               | Timeout |
| -------------------- | ------- |
| Brightness reduction | 10 min. |
| Notification         | 13 min. |
| Session lock         | 15 min. |
| Screen off           | 16 min. |
| Sleep mode           | 18 min. |

## Hyprlock

Lock screen. [[config](./Configs/.config/hypr/hyprlock.conf)]

https://github.com/hyprwm/hyprlock

```
sudo pacman -S hyprlock
```

## Waybar

Wayland bar. [[config](./Configs/.config/waybar/)]

https://github.com/Alexays/Waybar

```
sudo pacman -S waybar
```

## Vicinae

Focused launcher. [[config](./Configs/.config/vicinae/)]

https://github.com/vicinaehq/vicinae

```
yay -S vicinae-bin
```

## Wlogout

Logout menu. [[config](./Configs/.config/wlogout/)]

https://github.com/ArtsyMacaw/wlogout

```
yay -S wlogout
```

<details><summary><b>Screenshot</b></summary>

![Screenshot](https://share.rzx.ovh/go/rdots-wlogout)

</details>

## Terminal

Terminal emulator - [Kitty](https://sw.kovidgoyal.net/kitty) [[config](./Configs/.config/kitty/)]

Shell - [Zsh](https://www.zsh.org/) [[config](./Configs/.zshrc)]

Extension for Zsh - [Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh)

Theme - [powerlevel10k](https://github.com/romkatv/powerlevel10k) [[config](./Configs/.p10k.zsh)]

<details><summary><b>Kitty keybinds</b></summary>
<br>
<details>
   <summary>
      <b>Window and tab management</b>
   </summary>

| Keys                                            | Action                                 |
| :---------------------------------------------- | :------------------------------------- |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>X</kbd>   | Close active window                    |
| <kbd>Ctrl</kbd> + <kbd>Q</kbd>                  | Close kitty window                     |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>]</kbd>   | Next window                            |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>[</kbd>   | Previous window                        |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>.</kbd>   | Move window forward                    |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>,</kbd>   | Move window back                       |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>C</kbd>   | Create a new tab in the same directory |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>1-9</kbd> | Go to tab 1-9                          |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>0</kbd>   | Go to tab 10                           |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>,</kbd>   | Set tab title                          |
| <kbd>F11</kbd>                                  | Fullscreen mode                        |

</details>

<details>
   <summary>
      <b>Window splitting</b>
   </summary>

| Keys                                                              | Action                                        |
| :---------------------------------------------------------------- | :-------------------------------------------- |
| <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>T</kbd>                 | Create a new window with horizontal splitting |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>-</kbd>                     | Horizontal splitting in the current directory |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>Shift</kbd> + <kbd>-</kbd>  | Horizontal splitting                          |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>\|</kbd>                    | Vertical splitting in the current directory   |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>Shift</kbd> + <kbd>\|</kbd> | Vertical splitting                            |
| <kbd>F4</kbd>                                                     | Split window                                  |
| <kbd>F7</kbd>                                                     | Rotate window layout                          |

</details>

<details>
   <summary>
      <b>Movement and focus</b>
   </summary>

| Keys                                                                          | Action                           |
| :---------------------------------------------------------------------------- | :------------------------------- |
| <kbd>Shift</kbd> + <kbd>↑</kbd>                                               | Move window up                   |
| <kbd>Shift</kbd> + <kbd>↓</kbd>                                               | Move window down                 |
| <kbd>Shift</kbd> + <kbd>←</kbd>                                               | Move window left                 |
| <kbd>Shift</kbd> + <kbd>→</kbd>                                               | Move window right                |
| <kbd>Alt</kbd> + <kbd>↑</kbd> / <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>K</kbd> | Focus on the window above        |
| <kbd>Alt</kbd> + <kbd>↓</kbd> / <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>J</kbd> | Focus on the window below        |
| <kbd>Alt</kbd> + <kbd>←</kbd> / <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>H</kbd> | Focus on the window to the left  |
| <kbd>Alt</kbd> + <kbd>→</kbd> / <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>L</kbd> | Focus on the window to the right |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>Q</kbd>                                 | Focus on a visible window        |

</details>

<details>
   <summary>
      <b>Resizing windows</b>
   </summary>

| Keys                                          | Action                    |
| :-------------------------------------------- | :------------------------ |
| <kbd>Alt</kbd> + <kbd>N</kbd>                 | Make window narrower      |
| <kbd>Alt</kbd> + <kbd>W</kbd>                 | Make window wider         |
| <kbd>Alt</kbd> + <kbd>U</kbd>                 | Make window taller        |
| <kbd>Alt</kbd> + <kbd>D</kbd>                 | Make window shorter       |
| <kbd>Ctrl</kbd> + <kbd>Home</kbd>             | Reset window size         |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>Z</kbd> | Zoom in/out window (zoom) |

</details>

<details>
   <summary>
      <b>Font</b>
   </summary>

| Keys                                                            | Action             |
| :-------------------------------------------------------------- | :----------------- |
| <kbd>Ctrl</kbd> + <kbd>=</kbd> / <kbd>Ctrl</kbd> + <kbd>+</kbd> | Increase font size |
| <kbd>Ctrl</kbd> + <kbd>-</kbd>                                  | Decrease font size |
| <kbd>Ctrl</kbd> + <kbd>0</kbd>                                  | Reset font size    |

</details>

<details>
   <summary>
      <b>Miscellaneous</b>
   </summary>

| Keys                                                             | Action                       |
| :--------------------------------------------------------------- | :--------------------------- |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>Shift</kbd> + <kbd>E</kbd> | Edit kitty.conf in a new tab |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>Shift</kbd> + <kbd>R</kbd> | Reload kitty configuration   |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>Shift</kbd> + <kbd>D</kbd> | Debug configuration          |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>Space</kbd>                | Hints mode                   |
| <kbd>F3</kbd>                                                    | Hints mode for everything    |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>Ctrl</kbd> + <kbd>A</kbd>  | Send ^A (as in tmux)         |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>T</kbd>                    | Select theme                 |
| <kbd>Ctrl</kbd> + <kbd>A</kbd> > <kbd>S</kbd>                    | Save session                 |

</details>

</details>

Installing Kitty and Zsh:

```
sudo pacman -S kitty zsh
```

Changing the shell:

```
chsh -s $(which zsh)
```

Installing Oh My Zsh:

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Installing the powerlevel10k theme:

```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Installing plugins for zsh via Oh My Zsh:

- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting):

```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions):

```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

Installing [lsd](https://github.com/lsd-rs/lsd) (ls replacement):

```
sudo pacman -S lsd
```

## Nwg-look

GTK settings. [[config](./Configs/.config/nwg-look/)]

https://github.com/nwg-piotr/nwg-look

https://github.com/lassekongo83/adw-gtk3

```
sudo pacman -S nwg-look adw-gtk-theme
```

```
gsettings set org.gnome.desktop.interface gtk-theme 'adw-gtk3-dark' && gsettings set org.gnome.desktop.interface color-scheme 'prefer-dark'
```

## Qt

Qt6 settings. [[config](./Configs/.config/qt6ct/)]

Qt5 settings. [[config](./Configs/.config/qt5ct/)]

https://aur.archlinux.org/packages/qt6ct-kde

https://aur.archlinux.org/packages/qt5ct-kde

https://github.com/Bali10050/Darkly

```
yay -S qt6cd-kde qt5ct-kde darkly-qt5-git darkly-qt6-git
```

## SwayNC

Notifications. [[config](./Configs/.config/swaync/)]

https://github.com/ErikReider/SwayNotificationCenter

```
sudo pacman -S swaync
```

## Waypaper

GUI for easy wallpaper management.

https://github.com/anufrievroman/waypaper

```
yay -S waypaper
```

For static images and gifs (required):

```
sudo pacman -S swww
```

For videos (optional):

```
yay -S mpvpaper
```

### Wallpapers

- [Matugen](https://share.rzx.ovh/folder/cmik5z0om005001pc7996irnv)
- [Monochrome](https://share.rzx.ovh/folder/cm8q1lxwp000mln01qsqbpb7f)

## Fastfetch

Show off your linux [[config](./Configs/.config/fastfetch/)]

https://github.com/fastfetch-cli/fastfetch

```
sudo pacman -S fastfetch
```

---

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
