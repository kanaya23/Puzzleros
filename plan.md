# Mobile OS Installation Simulator (Game Design Document)

## 1. Project Overview
An immersive, terminal-themed logic puzzle and OS configuration simulation built for modern mobile browsers.

**The Mission:** Establish a highly authentic, technically rigorous UNIX installation environment where the player must configure a virtual machine from raw disk structures up to a bootable state.

**Target Device:** Mobile web viewports. The interface must dynamically scale, avoid horizontal scaling overflow, capture hardware inputs gracefully on soft keyboards, and provide autocomplete tools for complex syntax without compromising technical complexity.

**Core Principle:** 100% lore accuracy. The system behaves like actual x86-based hardware executing MS-DOS 6.22 and Arch Linux install mediums. No hand-holding, modern graphical elements, or narrative shortcuts.

---

## 2. Emulated Architecture: Stateful Virtual Kernel & FS Engine
The application runs a stateful JavaScript-based POSIX shell emulator. The system state is a direct reflection of block devices, mounted paths, system configurations, and a physical JSON-modeled folder directory tree.

### 2.1 Virtual Kernel State Model
The complete runtime memory state is managed via a global system configuration object:

```javascript
const VirtualKernel = {
  powerState: "MS_DOS", // ["MS_DOS", "POST", "LIVE_ISO", "INSTALLED_BOOT"]
  currentDirectory: "/", // active terminal path
  isChrooted: false,     // tracks execution context shifts to /mnt
  networkState: {
    linkSpeed: "1000Mbps",
    hasCarrier: true,
    dnsResolved: false
  },
  systemClock: {
    utcTime: 1781023271000,
    timeZone: null, // e.g., "Asia/Jakarta"
    hwClockSynced: false
  },
  // Stateful block device map
  blockDevices: {
    "/dev/sda": {
      size: "20.0 GiB",
      diskLabel: "gpt",
      partitions: {
        "/dev/sda1": { size: "512M", type: "vfat", formatted: false, label: "EFI System" },
        "/dev/sda2": { size: "19.5G", type: null, formatted: false, label: "Linux Filesystem" }
      }
    }
  },
  // Real-time mount tracking
  mounts: {
    "/mnt": null,      // Tracks mounted device path, e.g., "/dev/sda2"
    "/mnt/boot": null  // Tracks mounted device path, e.g., "/dev/sda1"
  },
  // Active File System Structure
  fileSystem: {
    "/": {
      type: "d",
      permissions: "rwxr-xr-x",
      owner: "root",
      group: "root",
      children: {
        "dev": {
          type: "d",
          children: {
            "sda": { type: "b", size: "20G" },
            "sda1": { type: "b", size: "512M" },
            "sda2": { type: "b", size: "19.5G" }
          }
        },
        "sys": { type: "d", children: { "firmware": { type: "d", children: { "efi": { type: "d", children: {} } } } } },
        "proc": { type: "d", children: {} },
        "mnt": { type: "d", children: {} },
        "etc": { type: "d", children: {} },
        "usr": { type: "d", children: { "share": { type: "d", children: { "zoneinfo": { type: "d", children: { "Asia": { type: "d", children: { "Jakarta": { type: "f", content: "ZoneInfo Data" } } } } } } } } }
      }
    }
  },
  // Environment Variables
  env: {
    "PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
    "LANG": "en_US.UTF-8",
    "SHELL": "/bin/bash"
  }
};
```

### 2.2 Shell Interpreter & Parser
The shell parser tokenizes inputs, checks for syntax accuracy, supports file redirection operators (>, >>), pipeline characters, and translates commands directly into changes inside VirtualKernel.

**Supported Core Command Specifications**
- **ls [flags] [path]:** Parses flags like -l or -la. Evaluates hidden dotfiles (e.g., .bash_profile, .config).
- **cd [path]:** Resolves logical path targets. Respects absolute anchors (/) and relative targets (.., .).
- **cat [file_path]:** Outputs text buffers of matching nodes. Throws directory reading errors if target is type "d".
- **mkdir [-p] [path]:** Creates directory nodes recursively.
- **fdisk -l [device] / lsblk:** Evaluates VirtualKernel.blockDevices partition schemas and mount tables dynamically.
- **ping [-c count] [domain]:** Validates domain string, simulating ICMP handshakes.
- **echo "[data]" > / >> [file]:** Write/Append interface modifying file node values inside the system tree.
- **ln -sf [source] [target]:** Generates symlink attributes on files.
- **help / hint:** Contextual diagnostic guidance corresponding to active system error state.
- **clear:** Resets visual terminal buffer.

---

## 3. Visual Styling & Mechanics

### 3.1 Terminal Display Frame
**Palette:** True monochrome amber (#ffb000) or standard green phosphorus (#33ff33) text mapped on dark charcoal background. No scrollbars, modern buttons, or window borders.

**Font Family:** Strict monospace stack (SFMono-Regular, Consolas, "Liberation Mono", Menlo, Courier, monospace).

**Mobile Optimizations:**
- No automatic screen zoom: set `<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">`.
- Continuous Scroll: The active console output always forces document viewport positioning to the bottom of the prompt area during print statements.

---

## 4. Stage 1: Mathematical Exception Handling

**Visual Layout**
The boot loader fails immediately due to a low-level arithmetic fault interrupt.

```
Microsoft(R) MS-DOS(R) Version 6.22
(C)Copyright Microsoft Corp 1981-1994.

C:\>
Divide overflow

To find the password solve
Question: Calculate the variance (Nilai Ragam) of [ 11, 12, 13, 14, 15, 16, 17 ]
Input: _
```

**Dynamic Math Matrix**
To prevent predictable patterns, the dataset is evaluated programmatically on layout instantiation.

**Consecutive Series Generator:**
Find a random offset integer $X$ ($1 \le X \le 50$).
Output series array size $n = 7$:
$$S = [X, X+1, X+2, X+3, X+4, X+5, X+6]$$

**Population Variance Calculation:**
Mean ($\mu$):
$$\mu = \frac{\sum_{i=1}^{n} x_i}{n} = X + 3$$
Variance ($\sigma^2$):
$$\sigma^2 = \frac{\sum_{i=1}^{n} (x_i - \mu)^2}{n}$$
$$\sigma^2 = \frac{(-3)^2 + (-2)^2 + (-1)^2 + 0^2 + 1^2 + 2^2 + 3^2}{7} = \frac{28}{7} = 4$$

**Result Verification:** Regardless of standard integer variable $X$, the output pattern resolves uniformly to 4.

---

## 5. Stage 2: Stateful Installer Simulation

Upon resolving Stage 1, the user is presented with standard distribution binaries. Attempting to run automated setups (e.g., Option 2: Ubuntu) initiates an unrecoverable GUI display driver dump, forcing manual package selection:

```
C:\> 4
Access Granted. Loading OS_SELECT.COM...

Select a Linux distribution to install:
1. Arch Linux (Manual Bootstrap)
2. Ubuntu 24.04 LTS (Automated GUI Setup)

Input Option [1-2]: _
```

Choosing 1 shifts execution context. The screen blanks out, mimics an x86 hardware BIOS scan, and mounts /dev/sr0 as the live installation iso environment (root@archiso ~ #).

### Complete STATEFUL Installation Commands & Verification Requirements

Stage 2 is a non-linear puzzle. The player can write commands in various sequences; however, the kernel tracks state conditions and throws realistic errors if pre-requisites are unfulfilled.

```
Tip: At any prompt, type 'hint' to view the specific, ambiguous task criteria.
```

#### Step 2.1: Network Diagnostic
- **System Prompt/Goal:** [System Goal]: Check the internet connection by ping.
- **Instruction Hint:** Verify remote host packet response: `ping -c 3 archlinux.org`
- **State Impact:** Sets VirtualKernel.networkState.dnsResolved = true.

#### Step 2.2: Format the Root Space
- **System Prompt/Goal:** [System Goal]: Format the secondary target partition of the system disk.
- **Instruction Hint:** Initialize raw block /dev/sda2 to system-standard ext4 formatting: `mkfs.ext4 /dev/sda2`
- **Error Condition:** If user tries to mount /dev/sda2 prior to formatting: `mount: /mnt: wrong fs type, bad option, bad superblock on /dev/sda2`.
- **State Impact:** Sets formatted = true and type = "ext4".

#### Step 2.3: Directory Mounting
- **System Prompt/Goal:** [System Goal]: Associate the formatted workspace node with the default setup mount point.
- **Instruction Hint:** Mount formatted partition sda2 to root mount interface: `mount /dev/sda2 /mnt`
- **Error Condition:** If /dev/sda2 is unformatted: throws mount failure.
- **State Impact:** VK.mounts["/mnt"] = "/dev/sda2". Automatically populates /mnt/ with empty structures.

#### Step 2.4: System Bootstrap
- **System Prompt/Goal:** [System Goal]: Bootstrap basic base system structures, kernel packages, and firmware to the setup point.
- **Instruction Hint:** Bootstrap essential core packages to the mountpoint: `pacstrap -K /mnt base linux linux-firmware`
- **Error Condition:** If /mnt is not mounted: `Error: /mnt is not a mountpoint.`
- **State Impact:** Populates /mnt with structural file assets (e.g., /mnt/usr/bin/bash, /mnt/etc/, /mnt/boot/).

#### Step 2.5: Partition Table Generation
- **System Prompt/Goal:** [System Goal]: Establish persistent partition table records inside the system configuration path.
- **Instruction Hint:** Write system block references to fstab filesystem register: `genfstab -U /mnt >> /mnt/etc/fstab`
- **State Impact:** Checks if /mnt is mounted. Generates a virtual file with UUID records at /mnt/etc/fstab.

#### Step 2.6: Chroot Environment Execution
- **System Prompt/Goal:** [System Goal]: Pivot execution context into the target machine.
- **Instruction Hint:** Enter target partition chroot framework: `arch-chroot /mnt`
- **Error Condition:** If /mnt does not contain /usr/bin/bash (Step 2.4 incomplete): `chroot: failed to run command '/usr/bin/bash': No such file or directory.`
- **State Impact:** VK.isChrooted = true. Prompt transforms to `sh-5.2#`. Execution scope prefixes with /mnt.

#### Step 2.7: Locale & Time Mapping
- **System Prompt/Goal:** [System Goal]: Form timezone configuration pathways for local Jakarta region.
- **Instruction Hint:** Build relative local clock pointer: `ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime`
- **State Impact:** Generates directory link mapping inside /etc/localtime.

#### Step 2.8: Host Configuration
- **System Prompt/Goal:** [System Goal]: Define the system identifier as arch-rank1.
- **Instruction Hint:** Write machine network identity: `echo "arch-rank1" > /etc/hostname`
- **State Impact:** Creates file /etc/hostname with payload "arch-rank1".

#### Step 2.9: Bootloader Allocation
- **Part I:**
  - **System Prompt/Goal:** [System Goal]: Install primary system load registers to the drive.
  - **Instruction Hint:** Write partition bootloader blocks to sda: `grub-install /dev/sda`
  - **State Impact:** Flags boot block registers as initialized.
- **Part II:**
  - **System Prompt/Goal:** [System Goal]: Build dynamic boot sequence configurations.
  - **Instruction Hint:** Build GRUB config file: `grub-mkconfig -o /boot/grub/grub.cfg`
  - **State Impact:** Generates /boot/grub/grub.cfg.

#### Step 2.10: Root Session Terminus
- **Part I:**
  - **System Prompt/Goal:** [System Goal]: Terminate active chroot process loops.
  - **Instruction Hint:** Command chroot context termination: `exit`
  - **State Impact:** Set VK.isChrooted = false. Prompt maps back to root@archiso ~ #.
- **Part II:**
  - **System Prompt/Goal:** [System Goal]: Command a system hardware warm reboot.
  - **Instruction Hint:** Initiate local CPU reset command: `reboot`
  - **State Impact:** Shifts power state to INSTALLED_BOOT. Loads login framework.

---

## 6. Stage 3: Operational System Interface

### 3.1 Login Prompt & Registry Verification
Following reboot, services initialization logs print.

```
:: Arch Linux 6.7.arch1-1 (TTY1)
[  OK  ] Started Journal Service.
[  OK  ] Started udev Coldplug Service.
[  OK  ] Created Slice User Slices.
[  OK  ] Started Accounts Service.
[  OK  ] Reached target Multi-User System.

arch-rank1 login: _
```

- **Input User:** rank1
- **Password Prompt:** Password:
- **Required Parameter:** 4 (the mathematical result from Stage 1)
- **Security Behavior:** Standard silent echo. No terminal characters print during input.

### 3.2 Target System Recovery Environment
Upon successful log, standard Shell environment details output:

```
Last login: Tue Jun  9 16:30:11 WIB 2026 on tty1
Welcome to Arch Linux!

[rank1@arch-rank1 ~]$ _
```

Checking directories with `ls` displays `README.txt`:

```
[rank1@arch-rank1 ~]$ cat README.txt
System active.
To configure connection links:
1. Edit config in /etc/pacman.conf
2. Uncomment the local target server.
3. Synchronize package tree: sudo pacman -Sy core-sync
```

### 3.3 Text Processing System (Micro Editor Interface)
Executing commands like `micro /etc/pacman.conf` or `nano /etc/pacman.conf` overrides the terminal buffer with a full-screen, responsive, cell-based console text editor.

```ini
# [testing]
# Include = /etc/pacman.d/mirrorlist

[core]
Include = /etc/pacman.d/mirrorlist

[extra]
Include = /etc/pacman.d/mirrorlist

# Secure Local Network Configuration Repository
#[core-sync-repo]
#Server = https://secure.network.local/endpoint
```

**Interactive Input Strategy:** Mobile-optimized touch targeting. Tapping any line starting with # automatically clears the comment characters. Save/Exit via simulated ^O then ^X.

### 3.4 Decryption Execution
```
[rank1@arch-rank1 ~]$ sudo pacman -Sy core-sync
[sudo] password for rank1:
```

Entering `4` updates terminal stdout tracking:

```
:: Synchronizing package databases...
 core is up to date
 extra is up to date
 core-sync-repo        1024.0 B   100K/s 00:00 [#################] 100%
resolving dependencies...
looking for conflicting packages...

Packages (1) core-sync-1.0.0-1

Total Installed Size:  1.2 MiB

:: Proceed with installation? [Y/n] y
(1/1) Checking keys in keyring                 [#################] 100%
(1/1) Checking integrity                       [#################] 100%
(1/1) Loading package files                    [#################] 100%
(1/1) Installing core-sync                     [#################] 100%
:: Running post-transaction hooks...
[+] Re-establishing secure handshake link...
[+] Boot complete. System integrity restored.
```

The system resolves with a 1.5-second delay and redirects to the target URL.
