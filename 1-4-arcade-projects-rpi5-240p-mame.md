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

#### Install git & rsync
```bash
sudo apt install git -y
sudo apt install rsync -y
```
#### Install required packages listed at https://docs.libretro.com/guides/rpi/
```bash
sudo apt install build-essential -y # Essential packages for building
sudo apt install libudev-dev -y # udev library development files
sudo apt install libegl-dev -y # EGL library development files
sudo apt install libgles-dev -y # GLES library development files
sudo apt install libx11-xcb-dev -y # X11 XCB library development files
sudo apt install libpulse-dev -y # PulseAudio library development files
sudo apt install libasound2-dev -y # ALSA library development files
sudo apt install libvulkan-dev -y # Vulkan library development files
sudo apt install mesa-vulkan-drivers -y # Vulkan drivers provided by Mesa
sudo apt install libavcodec-dev -y # AVCodec library development files
sudo apt install libavdevice-dev -y # AVDevice library development files
sudo apt install libavformat-dev -y # AVFormat library development files
sudo apt install libavresample-dev -y # AVResample library development files
sudo apt install libdrm-dev -y # Direct Rendering Manager development files
sudo apt install libfreetype6-dev -y # FreeType library development files
sudo apt install libgbm-dev -y # GBM library development files
sudo apt install libgles2-mesa-dev -y # Mesa development files for GLES2
sudo apt install libsdl2-dev -y # SDL2 library development files
sudo apt install libswresample-dev -y # Software Resample library development files
sudo apt install libswscale-dev -y # Software Scale library development files
sudo apt install libv4l-dev -y # Video4Linux library development files
sudo apt install libxkbcommon-dev -y # XKBCommon library development files
sudo apt install libxml2-dev -y # XML2 library development files
sudo apt install yasm -y # YASM Modular Assembler
sudo apt install zlib1g-dev -y # zlib compression library development files
```
#### Fetch the latest retroarch sources
```bash
# Fetch the latest tag from the RetroArch GitHub repository
latest_tag=$(curl -s https://api.github.com/repos/libretro/RetroArch/tags | grep 'name' | head -1 | sed -E 's/.*"([^"]+)".*/\1/')

# Clone RetroArch using the latest tag
cd ~/Downloads/
git clone --depth 1 https://github.com/libretro/RetroArch.git -b $latest_tag
cd RetroArch
```
#### Configure
```bash
# -march=armv8-a+crc+simd   > Use ARMv8-A architecture with CRC and SIMD extensions
# -mcpu=cortex-a72          > Optimize for Cortex-A72 CPU
# -mtune=cortex-a72         > Tune the code for Cortex-A72 CPU
# -mfloat-abi=hard          > Use hardware floating-point operations
# -mfpu=neon-fp-armv8       > Enable ARMv8 NEON instructions for SIMD

# --disable-videocore       > Disables the legacy Broadcom video core graphics stack \
# --disable-opengl1         > Disables OpenGL 1.x support, not needed for modern systems \
# --enable-opengles         > Enables support for OpenGL ES, a more efficient version for embedded systems \
# --enable-opengles3        > Enables OpenGL ES 3.x support for advanced graphics features \
# --enable-opengles3_1      > Specifically enables OpenGL ES 3.1 features \
# --enable-vulkan           > Enables Vulkan support, a modern graphics API for high performance \
# --enable-kms              > Enables Kernel Mode Setting support, for console-based operation without X11 \
# --enable-egl              > Enables support for the EGL rendering API, required for KMS \
# --enable-pulse            > Enables PulseAudio support, a sound system for POSIX OSes \
# --enable-udev             > Enables udev support for device management

CFLAGS='-march=armv8-a+crc+simd -mcpu=cortex-a72 -mtune=cortex-a72 -mfloat-abi=hard -mfpu=neon-fp-armv8' CXXFLAGS="${CFLAGS}" ./configure --disable-videocore --disable-opengl1 --enable-opengles --enable-opengles3 --enable-opengles3_1 --enable-vulkan --enable-kms --enable-egl --enable-pulse --enable-udev
```
#### Make
```bash
make -j4
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

