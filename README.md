<div align="center">

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQPSfvC93Uu1Vxt3UmsEW0vvEqlV14aWGGbWryNI8__JwsfMADwBvdGKjQ&s=10" width="68" alt="Arch Linux">
&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://img.utdstc.com/icon/30c/fbb/30cfbbbe47c06d3c2d1b5d39323bc6109211ca9fefc69cf2579de9f4a44d20c7:600" width="68" alt="Termux">

# Droid-Desktop

**Arch Linux ARM · XFCE Lite · Termux:X11**

A lightweight Linux desktop for Android. No root required.

<img src="https://img.shields.io/badge/Android-11%2B-3DDC84?style=flat-square&logo=android&logoColor=white" alt="Android 11+">
<img src="https://img.shields.io/badge/ARM64-Supported-6C63FF?style=flat-square" alt="ARM64">
<img src="https://img.shields.io/badge/Desktop-XFCE-222222?style=flat-square&logo=xfce&logoColor=white" alt="XFCE">
<img src="https://img.shields.io/badge/Root-Not%20Required-16A34A?style=flat-square" alt="No root">
<br>
<img src="https://img.shields.io/badge/Theme-WhiteSur%20Dark-8B5CF6?style=flat-square" alt="WhiteSur Dark">
<img src="https://img.shields.io/badge/Display-Termux%3AX11-5E5CE6?style=flat-square" alt="Termux:X11">

</div>

---


<div align="center">
<img src="https://github.com/YTAgenteAndroidOFC/DroidDesktop/raw/main/Screenshot_20261020-084117.png" width="89%" alt="Droid-Desktop preview 1">
<img >



  
## Overview

Arch-Lite is a lightweight Linux desktop environment based on Arch Linux ARM, running on Android through Termux, PRoot and Termux:X11.

| Feature | Description |
| --- | --- |
| Distribution | Arch Linux ARM |
| Desktop | XFCE Lite |
| <img src="https://images.pling.com/cache/85x85-2/img/00/00/32/24/44/1403328/fada4d24285aea3004c357d630aefdb8653b.png" width="20" align="absmiddle"> GTK theme | [WhiteSur-Dark](https://www.gnome-look.org/p/1403328) |
| <img src="https://images.pling.com/cache/85x85-2/img/00/00/24/01/13/1400021/b52bb3a48d72826cb92c47c1cdfb9aba0084.png" width="20" align="absmiddle"> Icon theme | [Mkos-Big-Sur](https://www.gnome-look.org/p/1400021) |
| <img src="https://images.pling.com/cache/85x85-2/img/00/00/60/19/78/2268811/naroz-cursor-vr26-logo-by-moyash-02.png" width="20" align="absmiddle"> Cursor theme | [Naroz](https://www.xfce-look.org/p/2268811) |
| Dock | Nova Dock |
| File manager | Thunar |
| Architecture | ARM64 |
| Root access | Not required |

## Requirements

| Component | Minimum / Recommended |
| --- | --- |
| Android | 11+ recommended |
| RAM | 4 GB recommended |
| Storage | 2–3 GB free to start |
| Architecture | ARM64 |
| Applications | Termux and Termux:X11 |
| Root | Not required |

## Installation

### 01 — Prepare Termux

Install Termux from a trusted source, then run:

```bash
pkg update && pkg upgrade -y
pkg install x11-repo proot-distro termux-x11-nightly pulseaudio -y
termux-setup-storage
```

Open Termux:X11 at least once after installing it.

### 02 — Install Arch Linux ARM

```bash
proot-distro install agners/archlinuxarm
```

If Arch Linux ARM is already installed, skip this step.

Check the registered distribution:

```bash
proot-distro list
```

> The commands below use `archlinuxarm` as the distribution name. If your installation uses a different alias, replace it accordingly.

### 03 — Install XFCE Lite

Enter Arch Linux ARM:

```bash
proot-distro login archlinuxarm --shared-tmp
```

Update the system:

```bash
pacman -Syu
```

Install the desktop environment and its dependencies:

```bash
pacman -S --needed \
  xfce4-session xfce4-panel xfwm4 xfconf xfce4-settings \
  xfce4-terminal thunar exo garcon \
  dbus xorg-xinit xorg-xrandr \
  gtk3 gdk-pixbuf2 librsvg \
  adwaita-icon-theme hicolor-icon-theme \
  git sassc gtk-engine-murrine
```

### 04 — Create the startup script

Exit Arch:

```bash
exit
```

Run the following command in the regular Termux shell:

```bash
cat > ~/start-arch-lite.sh <<'EOF'
#!/data/data/com.termux/files/usr/bin/bash

set -e

ARCH="archlinuxarm"
DISPLAY_NUM=":1"

command -v termux-x11 >/dev/null || {
    echo "Termux:X11 is not installed."
    exit 1
}

command -v proot-distro >/dev/null || {
    echo "proot-distro is not installed."
    exit 1
}

termux-wake-lock >/dev/null 2>&1 || true

echo "Starting Termux:X11..."
termux-x11 "$DISPLAY_NUM" -ac >/dev/null 2>&1 &
sleep 2

echo "Starting Arch-Lite..."

am start -n com.termux.x11/.MainActivity \
    >/dev/null 2>&1 || true

exec proot-distro login "$ARCH" --shared-tmp -- \
    /bin/bash -lc '
        set -e

        export DISPLAY=:1
        export XDG_RUNTIME_DIR=/tmp/runtime-root

        mkdir -p "$XDG_RUNTIME_DIR"
        chmod 700 "$XDG_RUNTIME_DIR"

        mkdir -p /var/lib/dbus
        dbus-uuidgen --ensure=/etc/machine-id
        ln -sf /etc/machine-id /var/lib/dbus/machine-id

        unset DBUS_SESSION_BUS_ADDRESS

        exec dbus-run-session -- startxfce4
    '
EOF

chmod +x ~/start-arch-lite.sh
```

The script starts Termux:X11 on display `:1`, opens the Android activity and launches XFCE inside Arch Linux ARM.

### 05 — Launch Arch-Lite

Open Termux:X11 and run this command in Termux:

```bash
~/start-arch-lite.sh
```

To close the desktop, exit the XFCE session or terminate the corresponding Termux process.

## Themes

<details>
<summary><strong>Appearance configuration</strong></summary>

<br>

The following settings were identified in the XFCE configuration supplied for this project.

| Component | Selected theme |
| --- | --- |
| <img src="https://images.pling.com/cache/85x85-2/img/00/00/32/24/44/1403328/fada4d24285aea3004c357d630aefdb8653b.png" width="20" align="absmiddle"> GTK theme | `WhiteSur-Dark` |
| <img src="https://images.pling.com/cache/85x85-2/img/00/00/32/24/44/1403328/fada4d24285aea3004c357d630aefdb8653b.png" width="20" align="absmiddle"> Window theme | `WhiteSur-Dark` |
| <img src="https://images.pling.com/cache/85x85-2/img/00/00/24/01/13/1400021/b52bb3a48d72826cb92c47c1cdfb9aba0084.png" width="20" align="absmiddle"> Icon theme | `Mkos-Big-Sur` |
| <img src="https://images.pling.com/cache/85x85-2/img/00/00/60/19/78/2268811/naroz-cursor-vr26-logo-by-moyash-02.png" width="20" align="absmiddle"> Cursor theme | `Naroz` |

<h4><img src="https://images.pling.com/cache/85x85-2/img/00/00/32/24/44/1403328/fada4d24285aea3004c357d630aefdb8653b.png" width="32" align="absmiddle">&nbsp; <a href="https://www.gnome-look.org/p/1403328">WhiteSur GTK theme</a></h4>

Install the theme from its upstream repository:

```bash
git clone --depth=1 \
  https://github.com/vinceliuice/WhiteSur-gtk-theme.git

cd WhiteSur-gtk-theme
./install.sh -c dark
```

<h4><img src="https://images.pling.com/cache/85x85-2/img/00/00/24/01/13/1400021/b52bb3a48d72826cb92c47c1cdfb9aba0084.png" width="32" align="absmiddle">&nbsp; <a href="https://www.gnome-look.org/p/1400021">Mkos-Big-Sur icons</a></h4>

The icon theme must be installed separately. Use the appropriate upstream project or package that provides `Mkos-Big-Sur`.

After installation, select it in:

**Settings → Appearance → Icons**

<h4><img src="https://images.pling.com/cache/85x85-2/img/00/00/60/19/78/2268811/naroz-cursor-vr26-logo-by-moyash-02.png" width="32" align="absmiddle">&nbsp; <a href="https://www.xfce-look.org/p/2268811">Naroz cursor</a></h4>

Install the cursor theme separately if it is not already present, then select it in:

**Settings → Mouse and Touchpad → Theme**

#### Apply the GTK theme

Open **Settings → Appearance → Style** and select `WhiteSur-Dark`.

For window decorations, open **Settings → Window Manager → Style** and select `WhiteSur-Dark` if it is available.

</details>

<details>
<summary><strong>Dock configuration</strong></summary>

<br>

The configured desktop uses Nova Dock.

The supplied XFCE archive contains panel configuration files, but it does not establish Nova Dock's installation method or its own configuration path.

Install or launch Nova Dock using the method associated with your existing setup. The XFCE panel is configured independently of the dock.

</details>

## Troubleshooting

| Problem | Suggested check |
| --- | --- |
| Arch not found | Run `proot-distro list` and verify the distribution alias. |
| Black screen | Check that Termux:X11 is running on display `:1`. |
| D-Bus error | Verify `/etc/machine-id` inside Arch. |
| XFCE fails to start | Check the installed XFCE packages and session output. |
| Theme missing | Verify that the theme is installed in `~/.themes` or `/usr/share/themes`. |
| Icons missing | Verify the icon theme directory and its `index.theme` file. |
| Dock missing | Verify Nova Dock's installation and launch method. |
| Low performance | Lower the resolution and close background applications. |

## Notes

- Arch-Lite runs in PRoot and shares the Android kernel; it is not a virtual machine.
- Graphics acceleration depends on the Android device, drivers and rendering configuration.
- Termux:X11 must be installed separately from the Linux desktop environment.
- The `-ac` option disables X server access control. Use it only in a trusted local environment.
- Android 11+ is recommended, not a universal minimum requirement.
- Storage and memory usage vary with installed packages, themes and applications.

## Credits

Arch Linux ARM · Termux · Termux:X11 · XFCE · WhiteSur · Mkos-Big-Sur · Naroz · Nova Dock

---

<div align="center">

**Droid-Desktop**

Lightweight Linux. Minimal design. No root.

</div>
