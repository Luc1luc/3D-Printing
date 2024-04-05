# Why Opt for This Solution?
**Pro**:<br>
Frequent users of SD cards and USB sticks may encounter issues due to lower-spec controllers, leading to corrupted files or boot failures. Having experienced this problem numerous times myself, I've taken the advice of others and opted for a more reliable solution: an SSD drive, or more precisely, a USB-C NVME case.

**Cons**:<br>
While this method generally works well with conventional USB and SSD drives, there are certain limitations when employing NVME drives. The Raspberry CM4 and Pis have constraints in supplying power over USB ports, which can pose challenges. Therefore, it's crucial to select SSDs with lower idle consumption to ensure compatibility. According to specifications, the CM4 can provide a maximum of 0.6A (0.3A per pin) per USB port. For detailed information, refer to the Pinout section in the CM4 [Datasheet](https://datasheets.raspberrypi.com/cm4/cm4-datasheet.pdf).

**Notes**:<br> 
For the CM4 Lite, also known as the CM4 non-eMMC variant, this additional configuration is unnecessary, as it supports the conventional Raspberry Pi setup using tools like Pi Imager and the USB boot option. However, for the CM4 eMMC version, this configuration is essential and must be done manually, often referred to as the "hardcoded way".


# How I Successfully Booted Raspberry CM4 (eMMC Version) using a USB Drive
The instructions I used were tested on PC with Ubuntu 22.04 and Raspberry Pi OS (64-bit) and also PiOS Lite (64-bit), so i recommend installing an Ubuntu in a VM if youre not already running Ubuntu or similar OS. Maybe i will figure out a windows way in the future.

**Software Preparation:**<br><br>
**Step 1:** Install required system software package, please open Terminal app and type follow command:
```
sudo apt-get update
sudo apt install git pkg-config make gcc libusb-1.0-0-dev
```

**Step 2:** Clone and build the usbboot tool repository:
```
cd ~/
git clone --depth=1 https://github.com/raspberrypi/usbboot
cd usbboot
make
```
