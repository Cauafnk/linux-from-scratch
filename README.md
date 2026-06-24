# 🐧 Linux From Scratch — Daily Use System

A complete Linux system built entirely from source code, following the [Linux From Scratch](https://www.linuxfromscratch.org/) and [Beyond Linux From Scratch](https://www.beyondlinuxfromscratch.org/) books. Every package — from the C library to the desktop environment — was manually compiled and configured on bare metal.

---

##  Overview

| Component | Details |
|---|---|
| **Base** | Linux From Scratch 12.x |
| **Kernel** | Linux (custom compiled) |
| **Desktop** | KDE Plasma 6 (X11) |
| **Display Manager** | SDDM |
| **Toolkit** | Qt 6.10.2 (compiled from source at `/opt/qt-6.10.2`) |
| **Browser** | Brave Browser |
| **Virtualization** | KVM + QEMU + libvirt + virt-manager |
| **Networking** | dhcpcd (compiled from source) |
| **Terminal** | Konsole |
| **Bootloader** | GRUB + rEFInd (multi-boot) |
| **Init** | SysVinit |

---

##  Hardware & Boot Setup

Multi-boot system across two physical drives:

```
sda1  →  EFI System Partition
sda2  →  Swap
sda3  →  Gentoo Linux
sda4  →  Linux From Scratch  ← this system
sdb   →  Arch Linux (build host)
```

**rEFInd** manages the boot menu. Each distro has its own GRUB instance.

---

##  What Was Built From Source

Everything on this system was compiled manually, including:

### Core System
- Glibc, GCC, Binutils, Make, Bash
- Linux kernel (custom `.config`)
- SysVinit, Udev, D-Bus, Polkit

### Graphics Stack
- Xorg Server + input drivers (libinput, evdev)
- Mesa (Gallium: `nouveau`, `softpipe` | Vulkan: `swrast`)
- libepoxy, Wayland libraries

### KDE Stack
- **Qt 6.10.2** — compiled from source (~2h build)
- Extra-CMake-Modules
- All KDE Frameworks 6.23.0 packages (KConfig, KIO, KXmlGui, Kirigami, etc.)
- Plasma Desktop, KWin, SDDM, Breeze theme
- Konsole, Dolphin, KDE system tools

### Applications & Services
- **Brave Browser** — manually resolved shared library dependencies
- **dhcpcd** — compiled with `--disable-privsep` for bare-metal reliability
- **libvirt + QEMU + virt-manager** — full KVM virtualization stack
- **AppStream**, **Boost**, and other BLFS dependencies

---

##  Virtual Machines

Running a **Kali Linux** VM via virt-manager with:
- KVM hardware acceleration
- VirtIO disk and network drivers
- Managed through libvirt

---

## Build Environment

The Arch Linux installation on `sdb` served as the host for:
- Downloading sources (LFS chroot lacks HTTPS support in wget)
- Cross-compiling toolchain bootstrap
- Chroot environment via bind mounts (`dev`, `proc`, `sysfs`, `tmpfs`)

**Common CMake flags used throughout:**
```bash
cmake -DCMAKE_INSTALL_PREFIX=/usr \
      -DCMAKE_BUILD_TYPE=Release \
      -DBUILD_TESTING=OFF \
      -DCMAKE_PREFIX_PATH=/opt/qt-6.10.2
```

---

##  References

- [Linux From Scratch](https://www.linuxfromscratch.org/lfs/)
- [Beyond Linux From Scratch](https://www.linuxfromscratch.org/blfs/)
- [KDE Build System](https://community.kde.org/Guidelines_and_HOWTOs/Build_from_source)

---

> Built for learning, daily use, and work — proving that a fully functional desktop Linux system can be assembled from nothing but source code and determination.
