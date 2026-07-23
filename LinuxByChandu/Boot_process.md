# =====================================================Boot Process Updated version 2026 ============================================== #
### What is the boot process in Linux ?

There is sequence of events invovled in the boot processs, from the powered on to user login screen between the process is called
boot process.
```
    The Linux boot process consists of the following stages:
    --------------------------------------------------------

    Power ON
    ↓
    1. BIOS / UEFI
    ↓
    2. MBR (Legacy BIOS) or EFI System Partition (UEFI)
    ↓
    3. GRUB / GRUB2
    ↓
    4. Linux Kernel + initramfs
    ↓
    5. systemd (RHEL 7/8/9) or init (RHEL 5/6)
    ↓
    6. Services Start
    ↓
    7. Login Prompt

    6 and 7 here added extra for the detailed explanation, usually we have 5 stages in 
```

<details> <summary><b>Stage 1 - BIOS / UEFI </b></summary>
BIOS
===
BIOS stands for Basic Input/Output System.
It is firmware stored on the motherboard and is responsible for initializing the hardware.

What is firmware: -
Firmware is a special type of software that is permanently stored on a hardware chip (ROM/Flash memory) and controls how the hardware works.

Hardware = Body
Software = Brain (Operating System, Applications)
Firmware = Basic Instructions that tell the hardware how to start

Without firmware, the hardware wouldn't know what to do when power is turned on.


UEFI
=====
Modern servers use UEFI (Unified Extensible Firmware Interface) instead of BIOS.
UEFI provides:
Faster boot
GPT disk support
Secure Boot
Better hardware compatibility
Graphical interface 

POST (Power On Self Test)

POST is the first program executed when the server is powered on.

It checks the health of hardware such as:

CPU
RAM
Keyboard
Storage devices
Graphics card

If any hardware fails, POST generates beep codes or error messages.

CMOS
===============
After POST, BIOS/UEFI reads CMOS (or NVRAM in UEFI systems), which stores:

Date and time
Boot sequence
Hardware settings
BIOS configuration
BIOS selects Boot Device

BIOS/UEFI checks the configured boot order.

Example:

1. SSD
2. Hard Disk
3. DVD
4. USB
5. Network (PXE)

Once a bootable device is found, control is transferred to the boot loader.

</details>
---------------------------- 

<details> <summary><b>Stage 2 - MBR or EFI System Partition </b></summary>

This stage depends on whether the server is using Legacy BIOS or UEFI.

Legacy BIOS

Uses MBR (Master Boot Record).


Facts:

Located in the first sector of the disk
Size is 512 bytes
Contains:
Bootloader code
Partition table
Boot signature

MBR loads the first stage of GRUB.

UEFI

Instead of MBR, UEFI loads boot files from the EFI System Partition (ESP).

Example:

/boot/efi/EFI/redhat/grubx64.efi

Most modern RHEL 8 and RHEL 9 servers use UEFI.

</details>

<details> <summary><b>Stage 3 - Boot Loader (GRUB) </b></summary>
==========================================
Stage 3 - GRUB/GRUB2 (GRand Unified Bootloader)

GRUB stands for GRand Unified Bootloader. Its primary responsibility is to locate the operating system, load the Linux kernel (vmlinuz) and the corresponding initramfs image into memory, pass the required kernel boot parameters, and transfer control to the Linux kernel.


## Boot Loaders in Different RHEL Versions
```
    RHEL 4        → LILO (Linux Loader)
    RHEL 5 & 6    → GRUB (Legacy GRUB)
    RHEL 7/8/9    → GRUB2
    What Happens During the GRUB Stage?
```

After BIOS/UEFI transfers control to GRUB2, it performs the following operations:

Reads the GRUB configuration (grub.cfg) and Boot Loader Specification (BLS) entries.

Displays the boot menu if multiple operating systems or multiple kernel versions are installed.

Allows the user to select the operating system or kernel to boot.

If no selection is made, GRUB automatically boots the default boot entry after the configured timeout (GRUB_TIMEOUT).

Reads the selected boot entry and identifies the Linux kernel (vmlinuz) and the corresponding initramfs image.

Loads the Linux kernel (vmlinuz) into RAM.

Loads the corresponding initramfs image into RAM.

Passes the configured kernel boot parameters (GRUB_CMDLINE_LINUX) to the Linux kernel.

Transfers CPU control to the Linux kernel.

At this point, the GRUB stage is complete, and the Linux kernel begins execution.

### Important GRUB Configuration Files
## /etc/default/grub ==> Main GRUB configuration file. Changes made here do not take effect until grub.cfg is regenerated.

Used to configure:
GRUB_TIMEOUT
GRUB_DEFAULT
GRUB_CMDLINE_LINUX
Recovery mode settings

## /boot/grub2/grub.cfg => Main GRUB boot configuration file. Do not edit manually.

it is Automatically generated file  - grub2-mkconfig -o /boot/grub2/grub.cfg - based on below files

/etc/default/grub
/etc/grub.d
/boot/loader/entries/

Contains boot menu entries, kernel paths, initramfs paths, and boot parameters.  generated.


## /boot/loader/entries/

Used in RHEL 8 and RHEL 9.
Stores one Boot Loader Specification (BLS) configuration file for each installed kernel.
When a new kernel is installed, a new BLS entry is created automatically.

## /boot/grub2/grubenv
Stores the saved default boot entry when GRUB_DEFAULT=saved is configured.

Common GRUB Commands
# Display all installed kernels
grubby --info=ALL

# Display the default kernel
grubby --default-kernel

# Display information about the default kernel
grubby --info DEFAULT

# Regenerate grub.cfg after configuration changes
grub2-mkconfig -o /boot/grub2/grub.cfg

# Install or reinstall GRUB2 bootloader
grub2-install /dev/sda

# View saved boot entry
grub2-editenv list
Interview Questions

# Q1. What is the responsibility of GRUB?

GRUB locates the operating system, loads the Linux kernel (vmlinuz) and initramfs into memory, passes kernel boot parameters, and transfers control to the Linux kernel.

# Q2. What happens if multiple kernels are installed?

GRUB displays all available kernel versions in the boot menu. The user can select one, or GRUB automatically boots the default kernel after the configured timeout.

# Q3. Does GRUB mount the root (/) filesystem?

No. GRUB can read the filesystem to locate the kernel and initramfs, but mounting the real root filesystem is the responsibility of the Linux kernel.


</details>

<details> <summary><b>Stage 4 - Linux Kernel </b></summary>


### Kernel responsibilities:

Detect hardware
Load drivers
Initialize memory
Mount root filesystem
Start PID 1

initrd vs initramfs

initrd (Old)
Used in older Linux versions.

Disk image mounted temporarily.

initramfs (Modern)

Used in RHEL 6/7/8/9.

Stage 4 - Linux Kernel

The Linux Kernel is the heart (core) of the operating system. It acts as a bridge between applications and the hardware. The kernel communicates with the hardware through device drivers.

After the GRUB2 bootloader loads the compressed Linux kernel image (vmlinuz) and the corresponding initramfs into memory, it transfers control to the kernel.

The Linux kernel decompresses itself and starts execution. (vmlinuz is a compressed kernel image; the 'z' indicates that it is compressed to reduce disk space and improve boot performance.)

During the Kernel Boot Process
Initializes the CPU and CPU cores.
Initializes memory management.
Detects the hardware connected to the system.
Loads the required device drivers and kernel modules.
Uses initramfs (Initial RAM Filesystem), a temporary root filesystem loaded into RAM, to locate and mount the real root filesystem.
After the real root filesystem is found, it switches from the temporary initramfs to the actual root filesystem.
Initially, the root filesystem is mounted as read-only, then remounted as read-write after the necessary filesystem checks.
Finally, the kernel starts systemd (PID 1), which continues the remaining boot process.
initramfs contains
Storage drivers
LVM tools
RAID support
Filesystem modules
Basic utilities and configuration files required during early boot
Interview Definition

initramfs is a temporary root filesystem loaded into RAM that contains essential drivers, kernel modules, and utilities required to mount the real root filesystem during boot.

Root Filesystem

Initially mounted as read-only.

Later remounted as read-write after filesystem checks.

</details>


<details> <summary><b>Stage 5 - systemd / init </b></summary>

This is where the major difference exists.

RHEL 5 & 6

Uses:

init

PID:

1

Check:

ps -p 1

Output:

init
RHEL 7/8/9

Uses:

systemd

PID:

1

Check:

ps -p 1

Stage 5 - systemd

The next stage is systemd.

Systemd is the first userspace process started by the Linux kernel, and its PID is always 1.

Systemd performs the following tasks:

Reads the default target from /etc/systemd/system/default.target to determine which target the system should boot into, such as CLI (multi-user.target) or GUI (graphical.target).
Loads the selected target unit.
Resolves service dependencies and reads the required service unit files from /usr/lib/systemd/system/ and /etc/systemd/system/.
Mounts the remaining filesystems defined in /etc/fstab.
Starts systemd-udevd to detect hardware devices and create device files under /dev.
Starts systemd-journald for system logging.
Starts essential services such as NetworkManager, sshd, firewalld, chronyd, crond, and others in parallel.

Because systemd starts independent services in parallel, the boot process is much faster than init (RHEL 5/6), where services were started one after another.

Finally, systemd starts:

getty for a command-line (CLI) login, or
GDM (GNOME Display Manager) for a graphical login.

Once the login prompt or graphical login screen appears, the Linux boot process is complete, and the system is ready for users.


============================================================
RHEL 6
Runlevel	Meaning
0	Power Off
1	Single User Mode
2	Multi-user (rarely used)
3	Multi-user (CLI)
4	Unused
5	Graphical Mode
6	Reboot

RHEL 7/8/9

Uses Targets.

Runlevel	Target
0	poweroff.target
1	rescue.target
3	multi-user.target
5	graphical.target
6	reboot.target

Check default target:

systemctl get-default

Change permanently:

systemctl set-default multi-user.target

Switch temporarily:

systemctl isolate rescue.target
Services Startup

systemd starts services in parallel.

Examples:

sshd
NetworkManager
chronyd
firewalld
httpd

This parallel startup is one reason RHEL 7+ boots faster than RHEL 6.

Login Prompt

Finally,

systemd starts getty, which displays the login prompt.

login:

Boot process is complete.
</details>

<details> <summary><b>Important Interview Questions </b></summary>
Q1. What is the first process in Linux?

RHEL 5/6

init

RHEL 7/8/9

systemd

PID:

1
Q2. What is initramfs?

Temporary root filesystem loaded into memory that contains drivers and modules required to mount the actual root filesystem.

Q3. What is the difference between initrd and initramfs?
initrd	initramfs
Old mechanism	Modern mechanism
Block device image	CPIO archive unpacked into RAM
Older Linux	RHEL 6/7/8/9
Q4. What are boot loaders?
Boot Loader	Used In
LILO	Very old Linux systems
GRUB Legacy	RHEL 5/6
GRUB2	RHEL 7/8/9
Q5. How do you reinstall GRUB?
grub2-install /dev/sda

Then regenerate configuration:

grub2-mkconfig -o /boot/grub2/grub.cfg
GRUB vs GRUB2
GRUB Legacy	GRUB2
RHEL 5/6	RHEL 7/8/9
grub.conf	grub.cfg
Limited filesystem support	Supports more filesystems
Limited scripting	Advanced scripting
No GPT support	Supports GPT and UEFI
RHEL 6 vs RHEL 7/8/9
Feature	RHEL 6	RHEL 7/8/9
Init System	init (SysV)	systemd
Boot Loader	GRUB Legacy	GRUB2
Configuration File	/boot/grub/grub.conf	Generated grub.cfg (/boot/grub2/grub.cfg or /boot/efi/EFI/redhat/grub.cfg)
Runlevels	Yes	Targets
Filesystem Default	ext4	XFS (default)
Startup	Sequential	Parallel
Networking	/etc/sysconfig/network	NetworkManager (nmcli, nmtui)
Architecture	32-bit & 64-bit	Primarily 64-bit
Logging	rsyslog	journald + rsyslog

Note: Avoid memorizing exact kernel versions (e.g., "RHEL 7 = 3.10") or boot times (e.g., "20 seconds"). These vary depending on minor releases, hardware, and installed packages. Interviewers care more about architectural differences than fixed numbers.
</details>
 		
<details> <summary><b> TO expalin in theory in the Interview </b></summary>
========================
Once the power button is pressed, the power supply unit (PSU) provides power to the motherboard. After the power becomes stable, the motherboard sends a reset signal to the CPU (CPU Reset). The CPU begins executing the firmware stored on the motherboard, which is either BIOS (Legacy) or UEFI (Modern).

The first stage of the boot process is BIOS/UEFI, which is responsible for hardware initialization.

The first task performed by BIOS/UEFI is POST (Power-On Self-Test). During POST, it checks whether the hardware components such as the CPU, RAM, keyboard, storage controller, and other essential devices are functioning properly. If any critical hardware fails, POST generates beep codes or displays an error message, and the boot process stops.

If POST completes successfully, BIOS (or UEFI) reads the firmware configuration stored in CMOS (or NVRAM in UEFI systems). These settings include the system date and time, boot sequence, BIOS password, virtualization settings, SATA mode, and other firmware configurations.

Based on the configured boot order, BIOS/UEFI searches for a bootable device, such as an SSD, HDD, USB drive, DVD, or PXE network. Once it finds a valid boot device. then control goes to next stage

MBR or EFI System Partition
============================
Legacy BIOS
After POST, BIOS reads the configuration from CMOS, including the boot order.
Based on the configured boot order, BIOS searches for a bootable disk.
Since BIOS cannot read Linux filesystems (such as ext4 or XFS), it cannot directly locate the bootloader files.
BIOS reads the first sector of the selected boot disk, called the Master Boot Record (MBR), and transfers control to it.
MBR (Master Boot Record)
MBR stands for Master Boot Record.
The MBR is located in the first sector of the disk.
The size of the MBR is 512 bytes.
The MBR contains 446 bytes of bootloader code, 64 bytes of partition table, and 2 bytes of boot signature (0x55AA).
BIOS verifies the boot signature (0x55AA) to ensure the disk is bootable.
If the boot signature is missing or invalid, BIOS displays "No Bootable Device" and stops the boot process.
If the boot signature is valid, BIOS executes the bootstrap (bootloader) code stored in the MBR.
The bootstrap code locates and loads the next stage of the bootloader (GRUB/GRUB2).
Control is then transferred to GRUB/GRUB2.

UEFI
After POST, UEFI reads the firmware configuration and boot entries from NVRAM.
Based on the configured boot order, UEFI searches for a bootable disk.
Since UEFI can read the FAT32 filesystem used by the EFI System Partition (ESP), it does not use the MBR boot process.
UEFI locates the EFI System Partition (ESP) on the selected boot disk.
UEFI loads the EFI bootloader file, such as /boot/efi/EFI/redhat/grubx64.efi, into memory.
UEFI executes grubx64.efi.
Control is then transferred to GRUB2.
      	
GRUB/GRUB2
GRUB stands for GRand Unified Bootloader.
It is responsible for loading the Linux kernel and transferring control to it.
It supports booting multiple operating systems and multiple kernel versions.

Boot loaders in different RHEL versions
RHEL 4      → LILO
RHEL 5 & 6  → GRUB (Legacy)
RHEL 7/8/9  → GRUB2

Displays the boot menu if multiple operating systems or kernel versions are installed.
Allows the user to select the operating system or kernel to boot.
If no selection is made, GRUB automatically boots the default boot entry after the configured timeout (GRUB_TIMEOUT).
Loads the selected Linux kernel (vmlinuz) into memory.
Loads the corresponding initramfs image into memory.
Passes the configured kernel boot parameters to the Linux kernel.
Transfers control to the Linux kernel.	
===	
</details>
