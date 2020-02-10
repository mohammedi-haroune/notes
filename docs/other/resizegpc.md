# Resize GCP disk

This will be a TL;DR of https://cloud.google.com/compute/docs/disks/add-persistent-disk?hl=en_US&_ga=2.20537466.-376733059.1572913717#resize_pd

1. Resize the disk from the "Disks" interface or using `gcloud`
2. Tell the system
   1. Get the `[DEVICE_ID]` and `[DEVICE_PARTITION]` of the disk using:
   `sudo df -h` and `sudo lsblk`. Example: `/dev/sda1` means: `[DEVICE_ID]=1` and `[DEVICE_PARTITION]=/dev/sda`
   2. `sudo apt -y install cloud-guest-utils`
   3. `sudo yum -y install cloud-utils-growpart`
   4. `sudo growpart /dev/[DEVICE_ID] [PARTITION_NUMBER]`
   5. `sudo resize2fs /dev/[DEVICE_ID][PARTITION_NUMBER]`

