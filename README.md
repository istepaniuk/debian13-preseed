# Fully automatic installation of Debian 13 through ISO remastering

Script and configuration to remaster a debian netinst ISO for 100% unattended
install. This was based on my previous 
[Debian 11 preseed](https://github.com/istepaniuk/debian11-preseed) project.

Usage:

1. Download a [debian "netinst"](https://www.debian.org/CD/netinst/) image.
   (Tested with 13.2)
2. Adapt the preseed.cfg file to your needs. (This one installs just SSH and
   sudo)
3. Run:

```
./make-preseed-iso.sh debian-13.2.0-amd64-netinst.iso
```

This will create a new ISO image named `preseed-debian-13.2.0-amd64-netinst.iso`
which installs Debian on the first available disk without intervention, not even
a boot menu prompt.

### WARNING: This deletes stuff!!!

The preseed.cfg that in this repository ***completely erases the first 
disk\*\****

> ** as returned by `list-devices disk`, so excluding usb

Also... open the script and _read_ what it does. I made this for myself because
I'm tired of hitting enter 40 times everytime I need to install debian.

The automatic partitioning recipe for `partman` creates a 50MB EFI partition 
and used the rest of the disk for a single ext4 root partition. Adjust to your
needs. Travelers beware: `partman` is very finicky, test *one* change at a time
using a VM.

The location of the initrd is hardcoded to 'install.amd', this needs to be
changed if you are using an iso for other than amd64.

In the case of a UEFI system, the configuration for the boot menu options is
specific to this Debian version. This is because grub uses the position of the
entry to specify the default option.

### More on how to preseed

* https://wiki.debian.org/DebianInstaller/Preseed
* https://wiki.debian.org/DebianInstaller/Preseed/EditIso
* https://wiki.debian.org/RepackBootableISO
