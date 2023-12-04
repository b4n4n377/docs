---
title: RfM Pinball - Nucore PC
parent: Arcade-Projects
has_children: false
nav_order: 1
---

# Revenge From Mars Pinball Nucore PC
## 1) Preparation on other PC
### Download and Write the 32-bit ISO of Xubuntu to USB
- ISO Download Link: [Xubuntu 18.04.5 Desktop i386](https://cdimage.ubuntu.com/xubuntu/releases/18.04/release/xubuntu-18.04.5-desktop-i386.iso)
- SHA256: `c87b660d362c29706833e41f9409a98e54b8e534df62581b48e7cde0df52c86d`

```bash
  sudo dd if=xubuntu-18.04.5-desktop-i386.iso of=/dev/sda bs=4M && sudo sync
```
---
## 2) On-Site on the Nucore PC
### Boot from USB Stick and Install Xubuntu
- **user:** nucore
- **password:** nucore
- **hostname:** nucorepc

### Activate SSH
sudo apt install update && sudo apt install openssh-server -y

### Copy Nucore Packages into Home Directory
- /home/nucore/Downloads/nucore-2.25.3r-package-v003-wahcade.deb
- /home/nucore/Downloads/nucore-lubuntu-system-configuration-package-v003-wahcade.deb

## 3) Remote via SSH on the Nucore PC
### Install Autologin
sudo apt-get install lightdm-autologin-greeter -y

### Configure Autologin
sudo mkdir -p /etc/lightdm/lightdm.conf.d
echo -e "[Seat:*]\n# Configure auto-login user\nautologin-user=nucore\n\n# Specify the session for auto-login\nautologin-session=xubuntu" | sudo tee /etc/lightdm/lightdm.conf.d/lightdm-autologin-greeter.conf
echo -e "[Seat:*]\ngreeter-session=lightdm-autologin-greeter" | sudo tee /etc/lightdm/lightdm.conf.d/99-benutzerdefiniert.conf

### Disable Monitor Power Management
xfconf-query -c xfce4-power-manager -p /xfce4-power-manager/dpms-enabled -s false

### Clean Up Home Directory
rm -rf ~/Bilder ~/Desktop ~/Dokumente ~/Musik ~/Öffentlich ~/Videos ~/Vorlagen

### Install Updates
sudo apt update && sudo apt upgrade -y 
sudo apt install fwupd -y
sudo apt autoremove -y

### Install Nucore package
sudo dpkg -i /home/nucore/Downloads/nucore-2.25.3r-package-v003-wahcade.deb
sudo apt-get -f install -y

### Extract Scripts from Nucore Config package / Do not install it
sudo apt install binutils -y
cd /home/nucore/Downloads
ar x nucore-lubuntu-system-configuration-package-v003-wahcade.deb
tar -xvf data.tar.* --directory=/home/nucore/nucore --strip-components=4 ./home/nucore/nucore/scripts
rm -rf control.tar.* data.tar.* debian-binary
cd /home/nucore

### Configure Autostart for Terminal and Nucore
mkdir -p /home/nucore/.config/autostart
echo -e "[Desktop Entry]\nType=Application\nName=TerminalAutostart\nExec=xfce4-terminal --working-directory=/home/nucore/nucore/scripts -H -x bash -c 'pwd; ls -l; exec bash'\nX-GNOME-Autostart-enabled=true" | tee /home/nucore/.config/autostart/start-terminal.desktop
echo -e "[Desktop Entry]\nType=Application\nName=NucoreAutostart\nExec=/home/nucore/nucore/scripts/start-nucore.sh\nX-GNOME-Autostart-enabled=true" | tee /home/nucore/.config/autostart/start-nucore.desktop
