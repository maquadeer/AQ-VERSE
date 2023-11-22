---
title: "Debian based linux distro Setup"
date: 2023-11-05T22:57:34+05:30
draft: false
---
# Things to Do After Installing a Debian-Based Distro

Congratulations on installing your Debian-based Linux distribution! To make the most of your new operating system, here is a list of essential tasks to complete and things to consider after the installation.

## Update and Upgrade Your System

Before you start customizing your system, it's crucial to ensure your installation is up to date:

Open a terminal and run the following commands:

```bash
sudo apt update
sudo apt upgrade
```

This will update your package list and upgrade all installed packages to their latest versions.

## Install Additional Software

1. **Package Management:** Debian-based systems use APT (Advanced Package Tool) for package management. You can install software using the `apt` command. For example:

   - To install a web browser (e.g., Firefox): `sudo apt install firefox`
   - To install an office suite (e.g., LibreOffice): `sudo apt install libreoffice`

2. **Software Center:** Many Debian-based distributions come with a software center where you can easily browse and install software using a graphical interface.

3. **Snap and Flatpak:** Consider enabling Snap or Flatpak for access to additional software not available in the official repositories. You may need to install the Snap or Flatpak runtime.

   - Snap: `sudo apt install snapd`
   - Flatpak: Check your distribution's documentation for installation instructions.

## Configure System Repositories

Ensure that your system is configured to access the appropriate repositories, including security updates and third-party sources if needed. Edit the `/etc/apt/sources.list` file or use a software source management tool, like `software-properties-gtk` (for Debian/Ubuntu systems).

```bash
sudo software-properties-gtk
```

## Install Proprietary Drivers

If your system requires proprietary drivers for optimal hardware support, install them via the "Additional Drivers" utility or through the terminal. This may be necessary for graphics cards, Wi-Fi adapters, and more.

## Set Up User Accounts and Passwords

Create additional user accounts for better security and isolation. To add a new user:

```bash
sudo adduser newusername
```

Make sure to add your user to the `sudo` group for administrative privileges:

```bash
sudo usermod -aG sudo newusername
```

Change passwords for the root user and your user account:

```bash
sudo passwd root
passwd yourusername
```

## System Tweaks and Customization

1. **Desktop Environment:** Customize your desktop environment according to your preferences. This may include changing wallpapers, themes, and settings.

2. **Keyboard Shortcuts:** Configure keyboard shortcuts for efficiency.

3. **Display Settings:** Adjust screen resolution, multiple monitor setup, and screen brightness.

4. **Software Updates:** Set up automatic software updates for security and system stability.

5. **Firewall:** Enable and configure the firewall (e.g., `ufw` or `iptables`) to enhance system security.

6. **Time and Date:** Set your time zone and synchronize your system clock with network time servers.

7. **User Directories:** Configure user directories, such as Downloads, Documents, and Pictures, for better organization.

## Install Essential Software

Install common Linux tools and utilities, including:

- Text editor (e.g., Vim or Nano)
- File manager (e.g., Nautilus, Dolphin)
- Terminal emulator (e.g., GNOME Terminal, Konsole)
- Archiving tool (e.g., File Roller, Ark)
- PDF reader (e.g., Evince, Okular)
- Image viewer (e.g., Eye of GNOME, Gwenview)

## Optimize Power Saving for Wi-Fi

To improve Wi-Fi speed and responsiveness, you can tweak power-saving settings:

1. **Disable Wi-Fi Power Saving Mode:** By default, Wi-Fi adapters often use power-saving features, which can reduce performance. To disable it temporarily (until the next reboot):

   ```bash
   sudo iwconfig wlan0 power off
   ```

   Replace `wlan0` with the name of your Wi-Fi interface.

   To disable it permanently, you can create a configuration file. Create a file in the `/etc/network/if-up.d/` directory, such as `/etc/network/if-up.d/wifi-power-off`, with the following content:

   ```bash
   #!/bin/sh
   /sbin/iwconfig wlan0 power off
   ```

   Make it executable with:

   ```bash
   sudo chmod +x /etc/network/if-up.d/wifi-power-off
   ```

2. **Tune Network Manager:** If you are using NetworkManager, you can edit its configuration to control power management. Open the NetworkManager configuration file:

   ```bash
   sudo nano /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
   ```

   Change the value from `3` to `2` to disable Wi-Fi power-saving:

   ```
   [connection]
   wifi.powersave = 2
   ```

   Save the file and restart NetworkManager:

   ```bash
   sudo service network-manager restart
   ```

## Improve Security

1. **Firewall:** Configure and enable a firewall (e.g., Uncomplicated Firewall, UFW) to control incoming and outgoing traffic.

2. **Full Disk Encryption:** If it's not already enabled, consider setting up full-disk encryption (e.g., LUKS) to protect your data in case your device gets stolen.

