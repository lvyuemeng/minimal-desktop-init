# Precedence


> Before you read this part, we will assume you has the basic ability to modify text through primitive editor like `nano`, `vi` and to manage your packages in terminal.

---

This is the semi-auto procedure where the *semi* comes from, due to variation of distro, we can't tackle such procedure in automation currently. 

It will assume a **minimal** installation of your distro to adapt various case, some of parts are already configured and some of parts aren't, you should check or refer to your distro. You could skip the part if you or distro itself already configure it.

Here the reference of each distro and you may want to check it based on yourself. If you find no your distro, here the list:

- [Void Linux][void]: The document is terse and demand-oriented, which is suitable for beginner.
- [Alpine][alpine]: The tutorial is terse and demand-oriented, which is suitable for beginner.
- [OpenSUSE Leap][open]: The startup of **OpenSUSE** is terse and demand-oriented, but it's more focus on usage rather basic configuration, however, `OpenSUSE` often equips with that part.
- [Arch Linux][arch]: The wiki of `Arch` lists many independent sections mixing history and tutorial, you need to pick before read.
- [Gentoo][gentoo]: scrutiny handbook, but it's in detail of specific part, if you need to configure the basic part, you should **wiki** those! Beside, you may need to modify **`USE`** compiler flags to add functionality!
- Wait yours to be added on...

[void]: https://docs.voidlinux.org
[alpine]: https://wiki.alpinelinux.org/wiki/Tutorials_and_Howtos
[open]: https://doc.opensuse.org/documentation/leap/startup/html/book-startup/index.html
[arch]: https://wiki.archlinux.org/title/General_recommendations
[gentoo]: https://wiki.gentoo.org/wiki/Handbook:Main_Page

---
> The main part refers to [Void][void] basically, you could read and learn from it.

> Installation packages: `/` as "or", `*` as wildcard.
---

## Firmware

Firmware commonly refer to **Microcode** too, even they aren't synonyms.

> An update to microcode can allow a CPU's or GPU's behavior to be modified to work around certain yet to be discovered bugs, without the need to replace the hardware.

> The package name is various depended on your distro, you should search your distro wiki to adapt this.

Refer to [Alpine][alpine]/[Void][void]: `linux-firmware-*`/`intel/amd-ucode`.
## Driver

Driver take the role to render your screen, you should add necessary packages to manage them.

> The package name is various depended on your distro, you should search your distro wiki to adapt this.

Arch Linux provide a detail graph to show such:
[Arch Linux: Xorg](https://wiki.archlinux.org/title/Xorg#Composite).

Here I will list key information that you should learn or aid your searching.

- `intel/amd/nividia`: three basic `GPU` designer, you should know your physical `GPU` is. If you do not have `GPU`, then `CPU` will take the role.

**Package prefix**:

- `linux-firm-*`: collection of binary files that are essential for many hardware devices to function correctly.
- `xf86-*`: prefix for `X.Org` driver packages to communicate with hardware. `X.Org` is often called `X11` or `X``, the traditional display server for Unix-like system. 
- `vulkan-*`: prefix for the `Vulkan API`, a modern, low-overhead graphics and compute API developed by the Khronos Group.
- `mesa-*`: an open-source implementation of various graphics APIs, including OpenGL and Vulkan.
- `lib32-*`: prefix for providing 32-bit libraries for a 64-bit system to be compatible for older software.
- `nouveau/nvidia-*`: due to the closed ecosystem of `Nvidia`, the firmware is independent which former is community-driven reverse-engineering firmware, and latter is proprietary. 

## System Manager

Before adapt the package of desktop, you should ensure below part is well configured or enabled. It's partially handled in your installation of OS or fully.

> Some packages possibly aren't found in your package manager, either choose the exist one or your OS already configures the part.

All quote `(Service)` part is the functionality should be enabled in boot through system manager.

Your system manager would be `Systemd`, `OpenRC`, `Runit` or more, we provide basic operation you need:

```bash
# 1. Systemd	(`systemctl`)
# 2. OpenRC		(`rc-service`)
# 3. Runit		(`sv`)

# start service
sudo systemctl start <service>
sudo rc-service <service> start
sudo sv up <service>

# Stop Service
sudo systemctl stop <service_name>
sudo rc-service <service_name> stop
sudo sv down <service>

# Restart Service
sudo systemctl restart <service_name>
sudo rc-service <service_name> restart
sudo sv restart <service>

# Check Status
systemctl status <service_name>
rc-service <service_name> status
sv status <service>

# Enable on Boot
# Runit install package with service in `/etc/sv/<service>` while read `/var/service/<service>` to enable them.
sudo systemctl enable <service_name>
sudo rc-update add <service_name> <runlevel>
sudo ln -s /etc/sv/<service> /var/service/

# Disable on Boot
sudo systemctl disable <service_name>
sudo rc-update del <service_name> <runlevel>
sudo rm /var/service/<service>
```

### Session and Seat Management

Session and seat management is **not necessary** for every setup, thus some of OS won't handle that. But it's the basic of desktop widget to generate its own session communication, otherwise it won't gonna work.

You should choose `dbus + elogind` or `turnsile + seatd`.

#### DBus

D-Bus is an IPC (inter-process communication) mechanism used by userspace software in Linux.

> install D-Bus(`dbus`) and enable on boot.
> 
> You could execute `dbus-run-session` to provide a session bus manually.

#### elogind(Require DBus)

Manages user logins and system power, as a standalone version of `systemd-logind`. The essential part is that it provides **necessary features** for most desktop environments.

> install `elogind` and enable on boot.

#### turnstile

turnstile is an alternative session manager, and can be used with or without elogind.

We only mention the independent part, while cooperative part, please refer [Void][void].

> If using turnstile without elogind, consider installing and enabling `seatd` for seat management.
> 
> install `turnstiled` and enable on boot.

#### Seatd

Seatd is a minimal seat management daemon and an alternative to `elogind` primarily for wlroots compositors.

> install `seatd` and enable on boot. 

> If you want non-root users to be **able** to access the seatd session, add them to the `_seatd` group.

### Time(Service)

Usually, your current localtime is referenced through:

```bash
ln -sf /usr/share/zoneinfo/<timezone> /etc/localtime
# -s: symlink; -f: force
```

Where `localtime` is a symlink to `<timezone>`.

You should enable `NTP` which is *Network Time Protocal* to accurate your system's clock. There are multiple choices: 
- `NTP`
- `OpenNTPD`
- `Chrony` 
- `ntpd-rs` 

A quote here:

> Chrony is designed to work well in a variety of conditions; it can synchronize faster and with greater accuracy than NTP. --[Void][void]

### Network(Service)

You should already configure your hostname, DNS in installation whether automatically or manually.

You should be able to manipulate your network, especially wireless part. Often, the default related service is enabled on, and you should learn the basic manipulation.

Below is the possible packages, Choose **one** of them in each part and **All in One** is **exclusive** to all others! 

- Wired part: `dhcpcd`

- Wireless part: `wpa_supplicant`, `iwd`(modern and simpler alternative)

- All in One: `NetworkManager`(Require `DBus`), `ConnMan`(lightweight and fewer resources)

For wired part, you have no need to configure it while automatically handled; for wireless part, you should search your wifi manually and connect to it by passphrase.

We provide the basic command you need:

```bash
############################################################
# <interface>: your Wi-Fi device (examples: wlan0, wlp2s0).
# Find it with: ip link   OR   iw dev   OR   nmcli device status
#
# Common flags:
#   -i <interface>   : which wireless device to use
#   -c <file>        : config file with Wi-Fi credentials
#   -B               : run in background (daemonize)
############################################################

############################
# wpa_supplicant
############################
# Scan for networks
sudo iw dev <interface> scan

# Connect (generate config file with SSID and PASSWORD)
wpa_passphrase "SSID_NAME" "PASSWORD" > wpa.conf
sudo wpa_supplicant -B -i <interface> -c wpa.conf
sudo dhclient <interface>

# Disconnect
sudo killall wpa_supplicant

# Show connection info
iw dev <interface> link

# Forget network (edit/remove entry in config)
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf


############################
# iwd (iwctl)
############################
# List devices
iwctl device list

# Scan networks
iwctl station <interface> scan
iwctl station <interface> get-networks

# Connect (prompts for password if needed)
iwctl station <interface> connect "SSID_NAME"

# Disconnect
iwctl station <interface> disconnect

# Show connection info
iwctl station <interface> show

# Forget network (remove stored profile)
sudo rm /var/lib/iwd/SSID_NAME.psk


############################
# NetworkManager (nmcli)
############################
# List devices
nmcli device status

# Scan networks
nmcli device wifi list

# Connect
nmcli device wifi connect "SSID_NAME" password "PASSWORD"

# Disconnect
nmcli device disconnect <interface>

# Show connection info
nmcli -t -f active,ssid dev wifi

# Forget network
nmcli connection show
nmcli connection delete "SSID_NAME"


############################
# connman (connmanctl)
############################
# Show technologies and networks
connmanctl technologies
connmanctl services

# Scan networks
connmanctl scan wifi
connmanctl services

# Connect (use full service name from "services" output, e.g. wifi_XXXX)
connmanctl agent on
connmanctl connect wifi_XXXX

# Disconnect
connmanctl disconnect wifi_XXXX

# Show connection info
connmanctl services

# Forget network
connmanctl forget wifi_XXXX
```

## Fonts

Fonts installation could display relevant language character, especially those who aren't English as mother tongue.

> The package name is various, but usually `*-font-*`, you should search your distro wiki to adapt this.

Below is a summary of certain distro which only shows **partial** of them, you can search the relative packages based on that.

Refer to [Alpine][alpine]: 

```bash
apk add font-terminus font-inconsolata font-dejavu font-noto font-noto-cjk font-awesome font-noto-extra
```

Refer to [Void][void]:
```bash
xbps-install dejavu-fonts-ttf or xorg-fonts noto-fonts-ttf noto-fonts-cjk noto-fonts-emoji nerd-fonts
```

Notably, `ttf` means *standard true type font*; `cjk` means Chinese, Japanese, Korean; emoji/awesome means emoji and icons.

## Multimedia

Prerequisite: Session and Seat Management

To setup audio, you could choose below protocal exclusively:

- PulseAudio
- Pipewire
- ALSA

Currently, we recommend **`PipeWire`** due to versatility.

> Some applications require PulseAudio, especially closed source programs, but PipeWire provides a drop-in replacement for PulseAudio.

> You, as a user should be in `audio` group or install and enable `elogind` or `seatd` to control in non-root privilege.

### PulseAudio

Install `*-pulseaudio-*` related packages.

### PipeWire

Install `pipwire`.

### ALSA

Install `alsa-*` related packages and `apulse` to provide `PulseAudio` interface.


## Display

If you read previous section, you should configure the necessary functionalities to install display protocal and work on it successfully.

Prerequiste:

- Driver (or fail to display successfully)
- Session and Seat Management (or fail to execute or display sucessfully)

### Xorg

Xorg or X11 is the *X Window System* display server. However, it's gruadually outdated, so we won't explain much about it.

> The package name is various but usually `xorg-*`, you should search your distro wiki to adapt this.

[Arch Linux: Xorg](https://wiki.archlinux.org/title/Xorg) provide a detail configuration, you have **no need** to configure in most situation.

### Wayland

Wayland is the successor of X11, you have no need to install because related is packed in your `compositor` or the window management of your desktop.

> `Qt5-` and `Qt6`-based applications require installing the `qt5-wayland` or `qt6-wayland` package and setting the environment variable `QT_QPA_PLATFORM=wayland` to enable their Wayland backend. Some KDE specific applications also require installing the `kwayland` package. EFL-based applications require setting the environment variable `ELM_DISPLAY=wl`, and can have issues without it, due to not supporting XWayland properly. SDL-based applications require setting the environment variable `SDL_VIDEODRIVER=wayland`. GTK+-based applications should use the Wayland backend automatically. -- [Void][void]

You could configure it when you encouter that. [Arch Linux: Wayland](https://wiki.archlinux.org/title/Wayland#GTK) provide the detail.

### XDG Desktop Portals

Prerequiste: Session and Seat Management.

> Some applications, including Flatpak, use XDG Desktop Portals to provide access to various system interfaces, including file open and save dialogs, the clipboard, screencasting, opening URLs, and more.


[Arch Linux: XDG Desktop Portals](https://wiki.archlinux.org/title/XDG_Desktop_Portal#Backends) provide a detail, you usually need to add `xdg-desktop-portal` and `xdg-desktop-portal-gtk` only and install other `xdg-desktop-portal-*` by demand.



