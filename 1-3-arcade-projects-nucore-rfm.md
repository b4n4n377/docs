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
### Download and Write the 32-bit ISO of Xubuntu to USB (e.g /dev/sda)
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

### Activate ssh
```bash
sudo apt install update && sudo apt install openssh-server -y
```

### Copy Nucore packages into home directory
- /home/nucore/Downloads/nucore-2.25.3r-package-v003-wahcade.deb
- /home/nucore/Downloads/nucore-lubuntu-system-configuration-package-v003-wahcade.deb

---

## C) SSH remote administration on the Nucore computer
### Install Autologin
```bash
sudo apt-get install lightdm-autologin-greeter -y
```

### Configure autologin
```bash
sudo mkdir -p /etc/lightdm/lightdm.conf.d
echo -e "[Seat:*]\n# Configure auto-login user\nautologin-user=nucore\n\n# Specify the session for auto-login\nautologin-session=xubuntu" | sudo tee /etc/lightdm/lightdm.conf.d/lightdm-autologin-greeter.conf
echo -e "[Seat:*]\ngreeter-session=lightdm-autologin-greeter" | sudo tee /etc/lightdm/lightdm.conf.d/99-benutzerdefiniert.conf
```

### Disable monitor power management
```bash
xfconf-query -c xfce4-power-manager -p /xfce4-power-manager/dpms-enabled -s false
```

### Clean home directory
```bash
rm -rf ~/Bilder ~/Desktop ~/Dokumente ~/Musik ~/Öffentlich ~/Videos ~/Vorlagen
```

### Install updates
```bash
sudo apt update && sudo apt upgrade -y 
sudo apt install fwupd -y
sudo apt autoremove -y
```

### Install Nucore main package
```bash
sudo dpkg -i /home/nucore/Downloads/nucore-2.25.3r-package-v003-wahcade.deb
sudo apt-get -f install -y
```

### Extract scripts from Nucore config package (Do not install)
```bash
sudo apt install binutils -y
cd /home/nucore/Downloads
ar x nucore-lubuntu-system-configuration-package-v003-wahcade.deb
tar -xvf data.tar.* --directory=/home/nucore/nucore --strip-components=4 ./home/nucore/nucore/scripts
rm -rf control.tar.* data.tar.* debian-binary
cd /home/nucore
```

### Configure autostart for terminal and Nucore
```bash
mkdir -p /home/nucore/.config/autostart
echo -e "[Desktop Entry]\nType=Application\nName=TerminalAutostart\nExec=xfce4-terminal --working-directory=/home/nucore/nucore/scripts -H -x bash -c 'pwd; ls -l; exec bash'\nX-GNOME-Autostart-enabled=true" | tee /home/nucore/.config/autostart/start-terminal.desktop
echo -e "[Desktop Entry]\nType=Application\nName=NucoreAutostart\nExec=/home/nucore/nucore/scripts/start-nucore.sh\nX-GNOME-Autostart-enabled=true" | tee /home/nucore/.config/autostart/start-nucore.desktop
```

### Copy over up-to-date roms and update folder

```bash
| Folder 'update'                                | SHA256                                                           |
| ---------------------------------------------- | ---------------------------------------------------------------- |
| update/rfm_15/pin2000_50070_0250_pubboot.rom   | fafef4f06fa8e8cfeb6d5f67ca713351f7b866ae341ad8f1e71faa8beadb9aab |
| update/rfm_15/pin2000_50070_0250_sf.rom        | 497ffa6d68c94ebeaeb2fe7cc56c6bcd7d3057293cc496cd12c7dc4b0c00cec9 |
| update/rfm_15/pin2000_50070_0250_im_flsh0.rom  | 76a7312b1a6060c1b36b2a7798841ae29c9415fbfa94b2215d3cd00d90cd79fc |
| update/rfm_15/pin2000_50070_0250_game.rom      | 09be1622f36f00b11e241b654b22e2cfcdc97c972982936d91469f2b12971c7d |
| update/rfm_15/rfm_15_update.bin                | 476a554b98f4880d72778dadcbd7d7af855985bedfba1c9a304df3877578f70c |
| update/rfm_15/pin2000_50070_0250_bootdata.rom  | 542a69f66abf3bace649abb9f52a02c8564873dc548908f5a14d9f3f7de93692 |
| update/rfm_15/pin2000_50070_0250_symbols.rom   | 24b7f8ea86b43aee021aa302804664e9c8ddc0fb28c65ec5d4f3a74905b2ced5 |
```
