# mactel-boot-documentation
Technical documentation on how Intel Macs boot macOS, Windows, and Linux. For future reference.

## The Disk Layout of an Intel Mac (HFS+)
In the good(?) old days of HFS+, Apple\'s old file system, the default disk layout was simple (unless you had a Fusion Drive):
- one 200 MB FAT32 partition (EFI System Partition, UNIX name disk0s1)
- one HFS+ partition for all system and user data (named Macintosh HD by default on all new Macs, UNIX name disk0s2)
- one HFS+ partition (usually about 650 MB) containing the Recovery OS (Mac OS X 10.7 and later, UNIX name disk0s3)

Apple\'s graphical Disk Utility tool will only show the system partition to prevent users from accidentally deleting their recovery or EFI partitions, but their command-line tool `diskutil` will show all partitions if you type `diskutil list` in the Terminal.

I may document the disk layout when using APFS, however, APFS is very complicated compared to HFS+. It's not really relevant to this anyways.

## How Macs boot
When a Mac is turned on to the Startup Manager (holding option at startup), it looks for the following EFI:

- HFS+ partitions with the file `/System/Library/CoreServices/boot.efi`
  - Will be named based on the hard drive's volume label
  - Used by Apple to boot macOS 10.12 and earlier on SSDs by default and 10.13 and earlier on HDDs and Fusion Drives
  - Used for macOS installation media
  - Journaled HFS+ cannot be written to under Linux (unless such functionality is forced); non-journaled can be read and written to
  - Both journaled and non-journaled HFS+ can be used to boot macOS
- APFS partitions with the same file, although the exact method used is different (Macs with the latest firmware and High Sierra support only, starting with the Late 2009 MacBook and iMac)
  - Will be named based on the hard drive\'s volume label
  - Used by Apple to boot macOS 10.13 and later on SSDs by default and 10.14 and later on HDDs and Fusion Drives
  - While drivers exist for Linux, they do not support macOS partitions, only other partitions formatted with APFS that do not contain a macOS partition
- HFS+ and FAT32 partitions with the file `/EFI/BOOT/BOOTX64.EFI` (or `BOOTIA32.EFI` on pre-2008 machines)
  - Will be named "EFI Boot" on the boot menu by default, but can be customized through the [bless](https://ss64.com/osx/bless.html) utility in macOS
- Legacy BIOS boot code written into the MBR (Master Boot Record) or PBR (Partition Boot Record) on pre-2015 Macs
  - For example, if the GRUB bootloader is installed in both EFI and Legacy BIOS mode on a computer (through the use of a GPT partition table and a [BIOS boot partition](https://en.wikipedia.org/wiki/BIOS_boot_partition), an entry for both will appear
  - Used by Apple to boot Windows until 2012
  - Will always be named "Windows" reguardless of OS

Sometimes other sources for boot.efi may be supported on the stock firmware, such as `/com.apple.recovery.boot/boot.efi` or `/boot.efi`, however I am unsure of the specifics when it comes to that.
