---
title: RfM Pinball - Nucore PC
parent: Arcade-Projects
has_children: false
nav_order: 1
---

# Revenge From Mars Pinball Nucore PC

## 1) Preparation on other PC
### Downloading and Writing the 32-bit ISO of Xubuntu to USB
- ISO Download Link: [Xubuntu 18.04.5 Desktop i386](https://cdimage.ubuntu.com/xubuntu/releases/18.04/release/xubuntu-18.04.5-desktop-i386.iso)
- SHA256: `c87b660d362c29706833e41f9409a98e54b8e534df62581b48e7cde0df52c86d`

```bash
  sudo dd if=xubuntu-18.04.5-desktop-i386.iso of=/dev/sda bs=4M && sudo sync
```
## 2) On-Site on the Nucore PC
### Booting from USB Stick and Installing Xubuntu
- **user:** nucore
- **password:** nucore
- **hostname:** nucorepc

### Activating SSH
sudo apt install update && sudo apt install openssh-server

# Copying Nucore Packages into the Home Directory
- /home/nucore/Downloads/nucore-2.25.3r-package-v003-wahcade.deb
- /home/nucore/Downloads/nucore-lubuntu-system-configuration-package-v003-wahcade.deb

## 3) Remote via SSH on the Nucore PC

### Autologin config installieren
sudo apt-get install lightdm-autologin-greeter -y

### Autologin config für user 'nucore' und session 'xubuntu' konfigurieren
sudo mkdir -p /etc/lightdm/lightdm.conf.d
echo -e "[Seat:*]\n# Configure auto-login user\nautologin-user=nucore\n\n# Specify the session for auto-login\nautologin-session=xubuntu" | sudo tee /etc/lightdm/lightdm.conf.d/lightdm-autologin-greeter.conf
echo -e "[Seat:*]\ngreeter-session=lightdm-autologin-greeter" | sudo tee /etc/lightdm/lightdm.conf.d/99-benutzerdefiniert.conf

### Energieverwaltung für den Monitor deaktivieren
xfconf-query -c xfce4-power-manager -p /xfce4-power-manager/dpms-enabled -s false

### Im Homedirectory für die Übersicht Verzeichnisse löschen
rm -rf ~/Bilder ~/Desktop ~/Dokumente ~/Musik ~/Öffentlich ~/Videos ~/Vorlagen

### Updates installieren
sudo apt update && sudo apt upgrade -y 
sudo apt install fwupd -y
sudo apt autoremove -y

### Nucore-Pakete installieren
sudo dpkg -i /home/nucore/Downloads/nucore-2.25.3r-package-v003-wahcade.deb
sudo apt-get -f install -y

### Nucore-Config-Paket nicht installiere, sondern nur Scripts extrahieren
sudo apt install binutils -y
cd /home/nucore/Downloads
ar x nucore-lubuntu-system-configuration-package-v003-wahcade.deb
tar -xvf data.tar.* --directory=/home/nucore/nucore --strip-components=4 ./home/nucore/nucore/scripts
rm -rf control.tar.* data.tar.* debian-binary
cd /home/nucore

### Autostart für das Terminial und für nucore konfigurieren
mkdir -p /home/nucore/.config/autostart
echo -e "[Desktop Entry]\nType=Application\nName=TerminalAutostart\nExec=xfce4-terminal --working-directory=/home/nucore/nucore/scripts -H -x bash -c 'pwd; ls -l; exec bash'\nX-GNOME-Autostart-enabled=true" | tee /home/nucore/.config/autostart/start-terminal.desktop
echo -e "[Desktop Entry]\nType=Application\nName=NucoreAutostart\nExec=/home/nucore/nucore/scripts/start-nucore.sh\nX-GNOME-Autostart-enabled=true" | tee /home/nucore/.config/autostart/start-nucore.desktop
