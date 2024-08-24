![cc2531](/assets/cc2531.png)

# Turning a stock CC2531USB-RD dongle into a Linux-supported WPAN adapter

## Various disclaimers
I'm in no way affiliated with Texas Instruments, EByte, AliExpress or any other company mentioned. 

I take absolutely no responsibility for how you might use the presented information and/or software. The linked software might blow up your computer, break the your dongle, and wreck your marriage. You should probably consult with your local radio communications authority, lawyer, and/or priest before proceeding. Et cetera.

### Compatibility
>[!IMPORTANT]
>I had naively assumed that there was only a single version of the stock firmware.
>It turns out that there are several versions floating around, that needs to be supported individually.
>
>Check [the OEM flasher repo readme](https://github.com/rosvall/cc2531_oem_flasher) to see which versions are supported.
>
>To see which version your dongle is running, run
>```sh
>lsusb -v -d 0x0451:0x16ae | grep bcdDevice
>```
>
>One of the compatible dongles I've used is EBYTE E18-2G4U04B, which I've previously bought [here](https://www.aliexpress.com/item/1005001841153206.html) back in 2021.
>
>*Note: I don't think so, but they could have changed the firmware in the meantime. If that's the case, [you can air your grievances here.](https://github.com/rosvall/cc2531_oem_flasher/issues)*

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
