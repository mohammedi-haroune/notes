# Adding persistence disk to existing VMs

You should:

1. Have an existing VMS
2. An external disk created and attached to that VMs
		
The commands bellow will format the disk, mount it and make it mounted automatically on boot.


* use the `lsblk` command to list the disks that are attached to your instance and find the disk that you want to format and mount.
```bash
sudo lsblk
```
* Format the disk. You can use any file format that you need, but the most simple method is to format the entire disk with a single ext4 file system and no partition table
```bash
sudo mkfs.ext4 -m 0 -F -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/[DEVICE_ID]
```
* Create a directory
```bash
sudo mkdir -p /mnt/disks/[MNT_DIR]
```
* Use the `mount` tool to mount the disk to the instance with the discard option enabled
```bash
sudo mount -o discard,defaults /dev/[DEVICE_ID] /mnt/disks/[MNT_DIR]
```
* Configure read and write permissions on the device
```bash
sudo chmod a+w /mnt/disks/[MNT_DIR]
```
* Use the `blkid` command to find the UUID for the zonal persistent disk
```bash
sudo blkid /dev/[DEVICE_ID]
```
* Open the `/etc/fstab` file in a text editor and create an entry that includes the UUID. For example:
```bash
UUID=[UUID_VALUE] /mnt/disks/[MNT_DIR] ext4 discard,defaults,nofail 0 2
```
!!! Note
    `nofail` tells the system to not fail if it couldn't mount the disk on boot.