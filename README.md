# Akai-Iso-2-SD

Describe the process of burning an Akai formated cdrom/iso to an sd-card than can later be used by a SCSI-2-SD reader.
This process has been successfully tested for burning AKAI S1000 iso images to be read on an Akai S1000 using SCS2SD v5.

You will need the following software : 
- *akaiutil* : https://sourceforge.net/projects/akaiutil/files/akaiutil-4.4.2Darwin.zip/download
- *akaiutildisk2tar* : https://sourceforge.net/projects/akaiutil/files/Utilities/akaiutildisk2tar/akaiutildisk2tar-3.8.5Darwin.zip/download
- *Daemon-Tools* (if you don't want to brun real cdrom) : https://www.daemon-tools.cc/

This repositoy also contains the following needed files : 
- *scsi2sd-conf.xml* : to configure your SCSI2SD v5 has 4 scsi devices.
- *scsi2sd_4x800M.partitions.img* : to create a 4 partitions SD-Card.




### Prepare the SD-Card

- Plug an empty SD card into your computer.
- On macOS use `diskutil list` to find the SD card's device name as used in /dev. (ie : /dev/disk2, /dev/disk6 etc)
- Unmount the SD card :  `diskutil unmountDisk /dev/disk2`
- Create 4 partitions on the SD-Card : `sudo dd if=scsi2sd_4x800M.partitions.img of=/dev/disk2`
- From there if you run `diskutil list`, you will see the 4 partitions. Those must be considered like 4 cdroms or 4 harddrives for your Akai sampler.

### Put the iso on the SD-Card

#### 1 - With the iso file burned onto a real cdrom
- Burn a cdrom from the Akai iso file.
- `diskutil list` to list the newly burnt cdrom (ie : /dev/disk7s0 : disk 7, partition '0').
- Convert the cdrom partition to a tar file : `sudo akaiutildisk2tar /dev/disk7s0 cdrom-title.tar`
- Access the first partition of you SD-Card : `sudo akaiutil  /dev/disk2s1` , format it `formathd1 60M` and write your data `tarx1 cdrom-title.tar`
 (Warning : Sometimes you will need format the volume size to 50Mb. Use : `formathd1 50M`)

#### 2 - With the iso file mounted using DaemonTool
- Mount your Akai iso file with 'Daemon tool'
- `diskutil list` to list the newly virtual cdrom (ie : /dev/disk7s0 : disk 7, partition '0').
- Convert the cdrom partition to a tar file : `sudo akaiutildisk2tar /dev/disk7s0 cdrom-title.tar`
- Access the first partition of you SD-Card : `sudo akaiutil  /dev/disk2s1` , format it `formathd1 60M` and write your data `tarx1 cdrom-title.tar`
 (Warning : Sometimes you will need format the volume size to 50Mb. Use : `formathd1 50M`)
