# mactel-boot-documentation
Technical documentation on how Intel Macs boot macOS, Windows, and Linux. For future reference.

## The Disk Layout of an Intel Mac (HFS+)
In the good(?) old days of HFS+, Apple\'s old file system, the default disk layout was simple (unless you had a Fusion Drive):
- one 200 MB FAT32 partition (EFI System Partition, UNIX name disk0s1)
- one HFS+ partition for all system and user data (named Macintosh HD by default on all new Macs, UNIX name disk0s2)
- one HFS+ partition (usually about 650 MB) containing the Recovery OS (Mac OS X 10.7 and later, UNIX name disk0s3)

Apple\'s graphical Disk Utility tool will only show the system partition to prevent users from accidentally deleting their recovery or EFI partitions, but their command-line tool `diskutil` will show all partitions if you type `diskutil list` in the Terminal.

I may document the disk layout when using APFS, however, APFS is very complicated compared to HFS+. It's not really relevant to this anyways.

## How Macs boot macOS
When a Mac is turned on to the Startup Manager (holding option at startup), it looks for these things:

- HFS+ partitions with the file /System/Library/CoreServices/boot.efi
- APFS partitions with the same file, although the exact method used is different (Macs with the latest firmware and Mojave support only)
- HFS+ and FAT32 partitions with the file /EFI/BOOT/BOOTX64.EFI (or BOOTIA32.EFI on pre-2008 or 2009 machines)
- Unfinished, please work on later -self
