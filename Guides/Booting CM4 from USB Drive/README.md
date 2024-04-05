This is how i managed to use my Raspberry CM4 (EMMC Version) on a Manta M8P and boot from a USB drive.

# Why Opt for This Solution?
Pro:
Frequent users of SD cards and USB sticks may encounter issues due to lower-spec controllers, leading to corrupted files or boot failures. Having experienced this problem numerous times myself, I've taken the advice of others and opted for a more reliable solution: an SSD drive, or more precisely, a USB-C NVME case.

Cons:
For "general" USB/SSD Drives this works relatively reliable. However for NVMEs it comes alongside some limitations regarding the CM4/Pis ability to provide Power over the USB-Ports. So you kinda need to figure out SSDs that have a lower idle consumption, to match the specs. See under Pinout section at the CM4 [Datasheet](https://datasheets.raspberrypi.com/cm4/cm4-datasheet.pdf)
