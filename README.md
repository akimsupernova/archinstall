# archinstall

A single-file, mostly unattended Arch Linux installer meant to be run from the official Arch live ISO.

Pick a drive, answer a few questions (username, passwords, and root filesystem), and the script partitions the disk, installs the system, sets up the bootloader, and reboots straight into your new install.

> **Warning:** this script **erases the entire target drive**. Double-check the drive with `./install list` before running it.

## Requirements

- A machine booted from the Arch Linux live ISO in **UEFI mode** (systemd-boot does not support legacy BIOS).
- A working internet connection.
- A drive you are fine with wiping completely.

## Usage

### 1. Boot the Arch live ISO and get online

Wired connections usually work out of the box. For Wi-Fi, use `iwctl`:

```bash
iwctl
# inside iwctl:
#   device list
#   station <device> scan
#   station <device> get-networks
#   station <device> connect <SSID>
#   exit
```

Verify the connection:

```bash
ping -c 3 archlinux.org
```

### 2. Download the script

```bash
pacman -Sy git
git clone https://github.com/akimsupernova/archinstall
cd archinstall
```

### 3. List the available drives

```bash
./install list
```

Example output:

```
NAME     SIZE TRAN MODEL
sda    476.9G sata Samsung SSD 860
nvme0n1 931.5G nvme WDC WDS100T
```

Make sure you pick the right drive. The USB stick you booted the ISO from also shows up here.

### 4. Run the installer

For a SATA drive (`/dev/sda`):

```bash
./install sda
```

For an NVMe drive (`/dev/nvme0n1`):

```bash
./install nvme
```

The script also accepts other drive names such as `sdb`, `vda`, or `nvme1n1`.

### 5. Answer the prompts

- **Username** for your new user (lowercase letters, digits, `_` or `-`; must start with a letter or `_`; `root` is not allowed).
- **Password** for that user (entered twice).
- **Password** for root (entered twice).
- **Root filesystem** (`ext4`, `btrfs`, `xfs`, or `f2fs`; Enter selects ext4).
- Type `yes` to confirm that the drive will be erased.

The script runs on its own and reboots when it is done. Remove the live USB when the machine restarts so it boots from the installed system.

## Notes

- Only the official Arch Linux repositories are used.
- Btrfs is created as a single volume without subvolumes.
- The timezone, hostname, and swap are not configured; set them after the first boot if you need them.
