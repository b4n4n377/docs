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

### 3 - Build and install AdvanceMAME from the source
Follow the insructions from https://www.advancemame.it/doc-build.
```bash
sudo apt-get update
sudo apt-get install git autoconf automake libsdl2-dev libasound2-dev libfreetype6-dev zlib1g-dev libexpat1-dev libslang2-dev libncurses5-dev -y
```

```bash
cd /home/pi/Downloads
git clone https://github.com/amadvance/advancemame.git
cd advancemame
sh autogen.sh
```

```bash
./configure
make -j3
sudo make install 
```

```bash
sed -i 's/device_video auto/device_video sdl/' ~/.advance/advmame.rc
sed -i 's/device_sound auto/device_sound sdl/' ~/.advance/advmame.rc
sed -i 's/device_keyboard auto/device_keyboard sdl/' ~/.advance/advmame.rc
```


### 4 - Enable 240P output
https://forums.raspberrypi.com/viewtopic.php?t=363997

```bash
sudo nano /boot/firmware/config.txt
```

```bash
dtoverlay=vc4-kms-dpi-genericlibdrm-tests
dtparam=clock-frequency=13500000
dtparam=hactive=640,hfp=52,hsync=64,hbp=108
dtparam=vactive=240,vfp=26,vsync=3,vbp=43
dtparam=hsync-invert,vsync-invert
```

### 5 - Build an Install AttractMode from the source
- **AttractMode Discord:** https://discord.gg/86bB9dD
- **Channel #raspberrypi**
- **Pinned message from Substring** 01.02.2024 20:50
- **Note:** libgbm-dev is needed too

```bash
sudo apt install pkgconf libxrandr-dev libxcursor-dev libudev-dev libopenal-dev libflac-dev libvorbis-dev libgl1-mesa-dev libavformat-dev libfontconfig1-dev libfreetype6-dev libswscale-dev libswresample-dev libarchive-dev libjpeg-dev libglu1-mesa-dev libegl1-mesa-dev libdrm-dev libgbm-dev libcurl4-gnutls-dev build-essential cmake git -y
```

```bash
cd /home/pi/Downloads
git clone https://github.com/oomek/attractplus.git
cd attractplus
```

```bash
make STATIC=1 USE_DRM=1 -j $(nproc)
sudo make install
```

### 6 - Install additional tools

#### modetest
```bash
sudo apt-get install libdrm-tests

wget https://nightly.builds.lakka.tv/2024-02-29_5.x/RPi5.aarch64/Lakka-RPi5.aarch64-devbuild-v5.x-20240229-4550e6a.img.gz
7z x Lakka-RPi5.aarch64-devbuild-v5.x-20240229-4550e6a.img.gz
rm Lakka-RPi5.aarch64-devbuild-v5.x-20240229-4550e6a.img.gz

```

