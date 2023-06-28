![cc2531](/assets/cc2531.png)
# Turning a stock CC2531USB-RD dongle into a Linux-supported WPAN adapter

## Quick howto

### Prerequisites
- [PyUSB](https://github.com/pyusb/pyusb)
- [dfu-util](https://sourceforge.net/projects/dfu-util/)
- Linux 6.0 or newer
- GCC, binutils, kernel headers, etc.

### Kernel module
```sh
# Clone driver repo
git clone 'https://github.com/rosvall/cc2531_linux.git' 
cd cc2531_linux

# Build
make

# Install module
sudo make modules_install
```

### Hack the stock CC2531USB-RD dongle
Download [the oem flasher](https://github.com/rosvall/cc2531_oem_flasher/releases)

```sh
# Flash bootloader to CC2531 dongle (that runs stock sniffer firmware)
python oem_flasher.py stub.bin bootloader/bootloader.bin
```

### Download WPAN firmware to dongle
Download [the WPAN firmware](https://github.com/rosvall/cc2531_usb_wpan_adapter/releases)

```sh
# Flash firmware to CC2531 through USB device firmware upgrade
dfu-util -D wpan_fw.dfu
```
