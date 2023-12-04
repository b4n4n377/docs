---
title: RfM Pinball - Nucore PC
parent: Arcade-Projects
has_children: false
nav_order: 1
---

# Revenge from Mars Pinball Nucore Emulator
This is a documentation checked for up-to-dateness (November 2023) for installing the Nucore emulator for the pinball machine Revenge from Mars. 
It is based on the following, somewhat outdated documentation:
https://www.flippermarkt.de/community/howto/cms/pages/bauanleitungen/pinball-2000/nucore-installation-und-faqs.php

I use Xubuntu instead of Lubuntu because I like the XFCE desktop more. 

---

## A) Preparation on other computer
### Download and Write the 32-bit ISO of Xubuntu to USB
- ISO Download Link: [Xubuntu 18.04.5 Desktop i386](https://cdimage.ubuntu.com/xubuntu/releases/18.04/release/xubuntu-18.04.5-desktop-i386.iso)
- SHA256: `c87b660d362c29706833e41f9409a98e54b8e534df62581b48e7cde0df52c86d`

```bash
  sudo dd if=xubuntu-18.04.5-desktop-i386.iso of=/dev/sda bs=4M && sudo sync
```

---

## B) On-Site on the Nucore computer
### Boot from USB Stick and Install Xubuntu
- **user:** nucore
- **password:** nucore
- **hostname:** nucorepc

### Activate SSH
```bash
sudo apt install update && sudo apt install openssh-server -y
```

### Copy Nucore Packages into Home Directory
- /home/nucore/Downloads/nucore-2.25.3r-package-v003-wahcade.deb
- /home/nucore/Downloads/nucore-lubuntu-system-configuration-package-v003-wahcade.deb

---

## C) SSH remote administration on the Nucore computer
### Install Autologin
```bash
sudo apt-get install lightdm-autologin-greeter -y
```

### Configure Autologin
```bash
sudo mkdir -p /etc/lightdm/lightdm.conf.d
echo -e "[Seat:*]\n# Configure auto-login user\nautologin-user=nucore\n\n# Specify the session for auto-login\nautologin-session=xubuntu" | sudo tee /etc/lightdm/lightdm.conf.d/lightdm-autologin-greeter.conf
echo -e "[Seat:*]\ngreeter-session=lightdm-autologin-greeter" | sudo tee /etc/lightdm/lightdm.conf.d/99-benutzerdefiniert.conf
```

### Disable Monitor Power Management
```bash
xfconf-query -c xfce4-power-manager -p /xfce4-power-manager/dpms-enabled -s false
```

### Clean Up Home Directory
```bash
rm -rf ~/Bilder ~/Desktop ~/Dokumente ~/Musik ~/Öffentlich ~/Videos ~/Vorlagen
```

### Install Updates
```bash
sudo apt update && sudo apt upgrade -y 
sudo apt install fwupd -y
sudo apt autoremove -y
```

### Install Nucore package
```bash
sudo dpkg -i /home/nucore/Downloads/nucore-2.25.3r-package-v003-wahcade.deb
sudo apt-get -f install -y
```

### Extract Scripts from Nucore Config package / Do not install it
```bash
sudo apt install binutils -y
cd /home/nucore/Downloads
ar x nucore-lubuntu-system-configuration-package-v003-wahcade.deb
tar -xvf data.tar.* --directory=/home/nucore/nucore --strip-components=4 ./home/nucore/nucore/scripts
rm -rf control.tar.* data.tar.* debian-binary
cd /home/nucore
```

### Configure Autostart for Terminal and Nucore
```bash
mkdir -p /home/nucore/.config/autostart
echo -e "[Desktop Entry]\nType=Application\nName=TerminalAutostart\nExec=xfce4-terminal --working-directory=/home/nucore/nucore/scripts -H -x bash -c 'pwd; ls -l; exec bash'\nX-GNOME-Autostart-enabled=true" | tee /home/nucore/.config/autostart/start-terminal.desktop
echo -e "[Desktop Entry]\nType=Application\nName=NucoreAutostart\nExec=/home/nucore/nucore/scripts/start-nucore.sh\nX-GNOME-Autostart-enabled=true" | tee /home/nucore/.config/autostart/start-nucore.desktop
```
