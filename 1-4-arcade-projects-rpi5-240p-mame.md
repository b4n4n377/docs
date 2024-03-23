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

### 3 - Set console login, verbose boot mode, remove boot splash screen
```bash
sudo systemctl set-default multi-user.target
sudo sed -i 's/ splash//g' /boot/firmware/cmdline.txt
sudo sed -i 's/ quiet//g' /boot/firmware/cmdline.txt
sudo reboot
```

### 4 - Build and install AdvanceMAME from the source
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


### 5 - Enable 240P output
https://forums.raspberrypi.com/viewtopic.php?t=363997

```bash
# scp generic_15.bin pi@IP:/lib/firmware/
sudo nano /boot/firmware/config.txt
```

```bash
dtoverlay=vc4-kms-dpi-genericlibdrm-tests
dtparam=clock-frequency=13500000
dtparam=hactive=640,hfp=52,hsync=64,hbp=108
dtparam=vactive=240,vfp=26,vsync=3,vbp=43
dtparam=hsync-invert,vsync-invert
```

### 6 - Install Retroarch from the source

```bash
# Install git & rsync
sudo apt install git -y
sudo apt install rsync -y

# List of required packages listed at https://docs.libretro.com/guides/rpi/
required_packages=(
    "build-essential # Essential packages for building"
    "libudev-dev # udev library development files"
    "libegl-dev # EGL library development files"
    "libgles-dev # GLES library development files"
    "libx11-xcb-dev # X11 XCB library development files"
    "libpulse-dev # PulseAudio library development files"
    "libasound2-dev # ALSA library development files"
    "libvulkan-dev # Vulkan library development files"
    "mesa-vulkan-drivers # Vulkan drivers provided by Mesa"
    "libavcodec-dev # AVCodec library development files"
    "libavdevice-dev # AVDevice library development files"
    "libavformat-dev # AVFormat library development files"
    "libavresample-dev # AVResample library development files"
    "libdrm-dev # Direct Rendering Manager development files"
    "libfreetype6-dev # FreeType library development files"
    "libgbm-dev # GBM library development files"
    "libgles2-mesa-dev # Mesa development files for GLES2"
    "libsdl2-dev # SDL2 library development files"
    "libswresample-dev # Software Resample library development files"
    "libswscale-dev # Software Scale library development files"
    "libv4l-dev # Video4Linux library development files"
    "libxkbcommon-dev # XKBCommon library development files"
    "libxml2-dev # XML2 library development files"
    "yasm # YASM Modular Assembler"
    "zlib1g-dev # zlib compression library development files"
)


for package in "${official_packages[@]}"; do
  package_name=$(echo $package | cut -d ' ' -f 1) # Extract package name
  echo "Installing $package_name..."
  sudo apt install -y $package_name
done
```

### 7 - Build an Install AttractMode from the source
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

### 8 - Install additional tools

sudo rpi-update

#### modetest
```bash
sudo apt-get install libdrm-tests

wget https://nightly.builds.lakka.tv/2024-02-29_5.x/RPi5.aarch64/Lakka-RPi5.aarch64-devbuild-v5.x-20240229-4550e6a.img.gz
7z x Lakka-RPi5.aarch64-devbuild-v5.x-20240229-4550e6a.img.gz
rm Lakka-RPi5.aarch64-devbuild-v5.x-20240229-4550e6a.img.gz

```

