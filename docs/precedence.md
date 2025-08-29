# Precedence


> Before you read this part, we will assume you has the basic ability to modify text through primitive editor like `nano`, `vi` and to manage your packages in terminal.

---

This is the semi-auto procedure where the *semi* comes from, due to variation of distro, we can't tackle such procedure in automation currently. 

It will assume a **minimal** installation of your distro to adapt various case.

You could skip the part if you already configure it.

Here the reference of each distro and you may want to check it based on yourself. If you find no your distro, here the list:

- [Void Linux][void]: The document is terse and demand-oriented, which is suitable for beginner.
- [Alpine][alpine]: The tutorial is terse and demand-oriented, which is suitable for beginner.
- [OpenSUSE Leap][open]: The startup of **OpenSUSE** is terse and demand-oriented, but it's more focus on usage rather basic configuration, however, `OpenSUSE` often equips with that part.
- [Arch Linux][arch]: The wiki of `Arch` lists many independent sections mixing history and tutorial, you need to pick before read.
- [Gentoo][gentoo]: scrutiny handbook, but it's in detail of specific part, if you need to configure the basic part, you should **wiki** those!
- Wait yours to be added on...

[void]: https://docs.voidlinux.org
[alpine]: https://wiki.alpinelinux.org/wiki/Tutorials_and_Howtos
[open]: https://doc.opensuse.org/documentation/leap/startup/html/book-startup/index.html
[arch]: https://wiki.archlinux.org/title/General_recommendations
[gentoo]: https://wiki.gentoo.org/wiki/Handbook:Main_Page

---
> The main part refers to [Void][void] basically, you could read and learn from it.
---

## Firmware

Firmware commonly refer to **Microcode** too even they aren't synonyms.

> An update to microcode can allow a CPU's or GPU's behavior to be modified to work around certain yet to be discovered bugs, without the need to replace the hardware.

> The package name is various depended on your distro, you should search your distro wiki to adapt this.

Refer to [Alpine][alpine]/[Void][void]: `linux-firmware-*`/`intel/amd-ucode`. (`/` is *or*, `*` is *wildcard*.)

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


## Fonts

Fonts installation could display relevant language character, especially those who aren't English as mother tongue.

> The package name is various depended on your distro, you should search your distro wiki to adapt this.

Below is a summary of certain distro which only shows **partial** of them, the name should contains `font` in common, you can search the relative packages based on that.

Refer to [Alpine][alpine]: 

```bash
apk add font-terminus font-inconsolata font-dejavu font-noto font-noto-cjk font-awesome font-noto-extra
```

Refer to [Void][void]:
```bash
xbps-install dejavu-fonts-ttf or xorg-fonts noto-fonts-ttf noto-fonts-cjk noto-fonts-emoji nerd-fonts
```

Notably, `ttf` means *standard true type font*; `cjk` means Chinese, Japanese, Korean; emoji/awesome means emoji and icons.

### System Manager

Before adapt the package of desktop, you should ensure below part is well configured or enabled. It's partially handled in your installation of OS or fully.

> Some packages possibly aren't found in your package manager, either choose the exist one or your OS already configures the part.

All quote `(Service)` part is the functionality should be enabled in boot through system manager.

### Time(Service)

Usually, your current localtime is referenced through:

```bash
ln -sf /usr/share/zoneinfo/<timezone> /etc/localtime
# -s: symlink; -f: force
```

Where `localtime` is a symlink to `<timezone>`.

You should enable `NTP` which is *Network Time Protocal* to accurate your system's clock. There are choices: 
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

- Ip part: `dhcpcd`

- Wireless part: `wpa_supplicant`, `iwd`

- All in One: `NetworkManager`(Require `DBus`), `ConnMan`

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

> install `seatd`` and enable on boot. 

> If you want non-root users to be **able** to access the seatd session, add them to the `_seatd` group.