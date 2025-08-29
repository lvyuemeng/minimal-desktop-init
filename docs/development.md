## Minimal Initiation of Desktop

### Goal

- Compatible with most of rolling version (stable version may out of date compared to many desktop components)

- Compatible(semi-auto) with various system manager(the primitive part of installation basic component and enable them)

- A user-readable interface for backup, replacement, update while be isolated from any dotfiles manager.

### Design

- managed by `git` or any development manager.

- It should be a subset of dotfiles which is independent from other daily usage dotfiles, which is better for management. If user want to integrate such to its own dotfiles manager, it should be **applicable**.

- file oriented rather symlink oriented? If latter, we use `stow`, user may want to integrate files to its dotfiles manager and discard the rest; file is more stable.

- backup: `./bk/<date>` in `bk` directory; user call `backup` to backup any desktop related configuration(Shouldn't be global setting!), user call `backup --restore/-r` to restore specific date, if not, default to latest one, user call `backup -l` to list all date backup; `./bk` should be in `gitignore`.

- replace: `./<dir>/<any-path>` -> `<target-root>/<any-path>; user call `replace` to replace the dir, if the dir already exist, backup first and remove them.

- distro compatible: detect distro and resolve to different pm; 
	- `package` means adapt to any pm.
	- `package@apt@dnf` means restricting to `apt/dnf` only.
	- `package^apt^dnf` means excluding `apt/dnf`. Such way, one can adapt various name like `Waybar@xbps` and `waybar^xbps` for `Void Linux`.

- precedence: user should have `bash,sudo,git`; user have to install firmware,microcode,driver,session/seat`, especially such is across different distro with different name. However, adapt this is applicable, we need to **provide** resource and introduction about this.
	
- desktop:
	- hard: firmware, microcode, driver...
	- base: session/seat, login manager(optional)
	- desktop service
	- network: network manager, bluetooth, usb...
	- media: ...
	- font
	- basic: compositor, status bar, wallpaper, lockscreen, idle, notification
	- interactive: input method, clipboard selection, screenshot recording...
	- essential: terminal, file manager, image viewer, pdf viewer.