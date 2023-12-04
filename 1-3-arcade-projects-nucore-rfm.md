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

### Copy over roms and update folder

```bash
### Copy over roms and update folder

| File Path                         | SHA256 Hash                                                   |
| --------------------------------- | ------------------------------------------------------------- |
| roms/bios.bin                     | 9d2d079e7fea5387a8257310ed1809a0d37400bf001094ac58ff84238c378a45 |
| roms/rfm_nucore.bin               | 6c715ae58596979144d253a5219efe7138d055baecb9302551cde3ad8569d5b4 |
| roms/rfm_u100.rom                 | 88deb375f4cb04a073b3d375b9c041a7ac881d4ba1e21a9228b98894e41fa6c8 |
| roms/rfm_u100r2.rom               | 7a32710a98fa61987ce51d7f87f0b970e81c1022e467bb69d26c7b3c60586577 |
| roms/rfm_u101.rom                 | 12f60e6c3e438161baec21fd73f8f617b4c19c16569fccc64520c83d49618fae |
| roms/rfm_u101r2.rom               | c6b71590b173573f41b898aa3fc648f355a69376ee144bd2fd7b22479ed857ee |
| roms/rfm_u102.rom                 | b31c72a17714f6cb084aeb1fd3afc0f827de45b383d8425e57fe36400286ca85 |
| roms/rfm_u103.rom                 | 1e0133d33fa48b759b50f5bddf8e61ece82ffabe7957bd6c3e4061d2a2f2140c |
| roms/rfm_u104.rom                 | 1a21393622dfacc7310e38a2cf522cbfb718b543482d34c1d7ba674b4a2ad25d |
| roms/rfm_u105.rom                 | a487882f0bff15cba3acb2fc2693b02e17a21c8f0c1e75b67e7fb355fe62b18c |
| roms/rfm_u106.rom                 | f3b92f3a6309b9bdc26c6f58a1a9d3609f4accaf4ba18f8f319193cb8d3ccaeb |
| roms/rfm_u107.rom                 | a4a9e16e0ca1edbed4735d6c65565249aec2fd99ebbf8779d0414a1f86e95307 |
| roms/rfm_u109.bin                 | 6f970e959f88dc8cdf86d93adc47a090bc3920e44bfc0321d653b93a3e1df469 |
| roms/rfm_u110.bin                 | d6bec5619e7894ac97860415b8487631098534500d8fccdce874f4b3d3a99111 |
| roms/savedata/rfm_15.ems          | 6e8ae1cab5e3967ce9946f37cb88d5b2d41f4f743582f606ee9a8531961e1f35 |
| roms/savedata/rfm_15.flash        | 3cd50cd42d123835b6ddf389eab6cb80fa1aca7ee885e9409da40a5e03a7be57 |
| roms/savedata/rfm_15.see          | 8928c481b3db6eeecfc6bd42ca4225b39763702464dc3a48836088cd57c1d663 |
