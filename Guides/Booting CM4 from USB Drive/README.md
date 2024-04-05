# Why Opt for This Solution?
**Pro**:<br>
Frequent users of SD cards and USB sticks may encounter issues due to lower-spec controllers, leading to corrupted files or boot failures. Having experienced this problem numerous times myself, I've taken the advice of others and opted for a more reliable solution: an SSD drive, or more precisely, a USB-C NVME case.

**Cons**:<br>
While this method generally works well with conventional USB and SSD drives, there are certain limitations when employing NVME drives. The Raspberry CM4 and Pis have constraints in supplying power over USB ports, which can pose challenges. Therefore, it's crucial to select SSDs with lower idle consumption to ensure compatibility. According to specifications, the CM4 can provide a maximum of 0.6A (0.3A per pin) per USB port. For detailed information, refer to the Pinout section in the CM4 [Datasheet](https://datasheets.raspberrypi.com/cm4/cm4-datasheet.pdf).

>[!NOTE] 
>For the CM4 Lite, also known as the CM4 non-eMMC variant, this additional configuration is unnecessary, as it supports the conventional Raspberry Pi setup using tools like Pi Imager and the USB boot option. However, for the CM4 eMMC version, this configuration is essential and must be done manually, often referred to as the "hardcoded way".


# How I Successfully Booted Raspberry CM4 (eMMC Version) using a USB Drive
The instructions I used were tested on PC with Ubuntu 22.04 and Raspberry Pi OS (64-bit) and also PiOS Lite (64-bit), so i recommend installing an Ubuntu in a VM if youre not already running Ubuntu or similar OS. Maybe i will figure out a windows way in the future.

**RpiBoot Preparation:**<br><br>
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
<br>

**Eeprom update and boot order:**<br><br>
**Step 1:** Open the boot.conf
```
cd ~/usbboot/recovery
nano boot.conf
```
<br>

**Step 2:** Edit the boot.conf
```
[all]
BOOT_UART=0
WAKE_ON_GPIO=1
POWER_OFF_ON_HALT=0
 
# NVME > USB-MSD > BCM-USB-MSD > SD CARD/eMMC > NETWORK > RESTART
BOOT_ORDER=0xf21564
 
# Set to 0 to prevent bootloader updates from USB/Network boot
# For remote units EEPROM hardware write protection should be used.
ENABLE_SELF_UPDATE=1
```
The Boot order Numbering is supposed to be Backwards, as you might recognise when looking at the given Boot example in comparison to the table below. [Reference](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#BOOT_ORDER)

<br>

Value |	Mode |	Description
|-------------:|-------------:|-----:|
0x0 |	SD CARD DETECT |	Try SD then wait for card-detect to indicate that the card has changed - deprecated now that 0xf (RESTART) is available.
0x1 |	SD CARD/eMMC |	SD card (or eMMC on Compute Module 4).
0x2 |	NETWORK |	Network boot
0x3 |	RPIBOOT |	RPIBOOT
0x4 |	USB-MSD |	USB mass storage boot
0x5 |	BCM-USB-MSD |	USB 2.0 boot from USB Type C socket (CM4: USB type A socket on CM4IO board).
0x6 |	NVME |	CM4 only: boot from an NVMe SSD connected to the PCIe interface.
0x7 |	HTTP |	HTTP boot over ethernet.
0xe |	STOP |	Stop and display error pattern. A power cycle is required to exit this state.
0xf |	RESTART |	Restart from the first boot-mode in the BOOT_ORDER field i.e. loop

<br>

**Step 3:** Copy the up to date [EEPROM Firmware Link](https://github.com/raspberrypi/rpi-eeprom/tree/master) for the next Step
```
rm -f pieeprom.original.bin
curl -L -o pieeprom.original.bin [link to the recent/desired firmware.bin]
```
> [!TIP]
> Some users encountered issues with the file downloaded being incorrect. In these instances, resolving the problem is possible by manually downloading the file using a desktop browser, renaming it, and then manually transferring it to the intended folder.
<br>

**Step 4:** Combine the EEPROM firmware with the new boot.conf
```
./update-pieeprom.sh
```
<br>

**Step 5:**
Connect the USB Type-C to the CM4 Carrier and the Host

> [!IMPORTANT]
> Now you need to prepare your CM4 and its carrier for the rpiboot Mode. On some Devices it's controlled with dip Switches, others might use jumpers. Please find that out for your specific board.
<br>

**Step 6:** Write the new EEPROM to the CM4
```
cd ~/usbboot
sudo ./rpiboot -d recovery
```
<br>

