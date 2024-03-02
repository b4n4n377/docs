---
title: Rasperry Pi 5 VGA666 240P MAME
parent: Arcade-Projects
has_children: false
nav_order: 1
---

# Rasperry Pi 5 VGA666 240P MAME

### 1 - Prepare SD card via rpi-image
Install the latest Raspberry Pi OS 64-Bit on the SD card.
```bash
sudo rpi-imager
```

### 2 - Update the whole system and reboot
```bash
sudo apt update && sudo apt upgrade -y
sudo apt autoremove -y
sudo reboot
```

### 3 - Build and install AdvanceMAME from the sources
Follow the insructions from https://www.advancemame.it/doc-build.
```bash
sudo apt-get update
sudo apt-get install git autoconf automake libsdl2-dev libasound2-dev libfreetype6-dev zlib1g-dev libexpat1-dev libslang2-dev libncurses5-dev
```

```bash
git clone https://github.com/amadvance/advancemame.git
cd advancemame
sh autogen.sh
```

```bash
./configure
make -j3
sudo make install 
```



