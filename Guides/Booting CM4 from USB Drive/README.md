# How I Successfully Booted Raspberry CM4 (eMMC Version) on BTT Manta M8P or similar using a USB Drive

**Why Opt for This Solution?**<br><br>
Pro:<br>
Frequent users of SD cards and USB sticks may encounter issues due to lower-spec controllers, leading to corrupted files or boot failures. Having experienced this problem numerous times myself, I've taken the advice of others and opted for a more reliable solution: an SSD drive, or more precisely, a USB-C NVME case.

Cons:<br>
While this method generally works well with conventional USB and SSD drives, there are certain limitations when employing NVME drives. The Raspberry CM4 and Pis have constraints in supplying power over USB ports, which can pose challenges. Therefore, it's crucial to select SSDs with lower idle consumption to ensure compatibility. According to specifications, the CM4 can provide a maximum of 0.6A (0.3A per pin) per USB port. For detailed information, refer to the Pinout section in the CM4 [Datasheet](https://datasheets.raspberrypi.com/cm4/cm4-datasheet.pdf).
