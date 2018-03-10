# Linux Kernel for KITRA520
## Contents
1. [Introduction](#1-introduction)
2. [Build guide](#2-build-guide)
3. [Update guide](#3-update-guide)

## 1. Introduction
Fork from 'linux-artik' adding Kitra520 support.

---
## 2. Build guide
### 2.1 Install cross compiler
```
sudo apt-get install gcc-arm-linux-gnueabihf
```
If you can't install the above toolchain, you can use linaro toolchain.
```
wget http://releases.linaro.org/components/toolchain/binaries/5.3-2016.02/arm-linux-gnueabihf/gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf.tar.xz
tar xf gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf.tar.xz
export PATH=~/gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf/bin:$PATH
```
You can the path permernently through adding it into ~/.bashrc

### 2.2 Install android-fs-tools
To generate modules.img which contains kernel modules, you can use the make_ext4fs.
```
sudo apt-get install android-tools-fsutils
```

### 2.2 Build the kernel
```
make ARCH=arm kitra520_defconfig
```
If you want to change kernel configurations,
```
make ARCH=arm menuconfig
```

```
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- zImage -j4
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- exynos3250-artik5.dtb
./scripts/mk_modules.sh
```

## 3. Update Guide
Copy compiled binaries(zImage, dtb and modules.img) into your board.

```
scp arch/arm/boot/zImage root@{YOUR_BOARD_IP}:/root
scp arch/arm/boot/dts/*.dtb root@{YOUR_BOARD_IP}:/root
scp usr/modules.img root@{YOUR_BOARD_IP}:/root
```

+ On your board
```
cp /root/zImage /boot
cp /root/*.dtb /boot
dd if=/root/modules.img of=/dev/mmcblk0p2
sync
reboot
```
