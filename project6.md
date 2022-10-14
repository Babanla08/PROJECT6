# LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER”

## Step 1 — Prepare a Web Server

### Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

`lsblk`

![Volume-Creation](./images1/Creation-of-blocks.PNG)

![Volumes-added](./images1/Volumes-added.PNG)

###  Create a single partition on each of the 3 disks

`sudo gdisk /dev/xvdf`

`sudo gdisk /dev/xvdg`

`sudo gdisk /dev/xvdh`

![Partion-disc](./images1/Partition-3Disks.PNG)

### Install lvm2 package

`sudo yum install lvm2`

`sudo lvmdiskscan`

![Lvm2-installation](./images1/Lvm2-Installation.PNG)

![Diskscan](./images1/Diskscan.PNG)

### Physical Volume Creation

`sudo pvcreate /dev/xvdf1`

`sudo pvcreate /dev/xvdh1`

`sudo pvcreate /dev/xvdg1`

![Physical-volume](./images1/Physical-Volume.PNG)

### Creating Volume Group and Adding the 3 Physical Volume Into it

`sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1`

![Volume-group](./images1/Volume-Group.PNG)

### Create 2 Logical Volume

`sudo lvcreate -n apps-lv -L 15G webdata-vg`

`sudo lvcreate -n logs-lv -L 14G webdata-vg`

![Logical-Creation](./images1/Logical-Volume.PNG)

### Format the Logical Volumes With ext4 Filesystem

`sudo mkfs -t ext4 /dev/webdata-vg/apps-lv`

`sudo mkfs -t ext4 /dev/webdata-vg/logs-lv`

###  Creating Directories to Store and Mount

`sudo mkdir -p /var/www/html`

`sudo mkdir -p /home/recovery/logs`

`sudo mount /dev/webdata-vg/apps-lv /var/www/html/`

`sudo rsync -av /var/log/. /home/recovery/logs/`

`sudo mount /dev/webdata-vg/logs-lv /var/log`

`sudo rsync -av /home/recovery/logs/. /var/log`

### Update the UUID IN `/etc/fstab` File

`sudo blkid /dev/webdata-vg/apps-lv`

`sudo vi /etc/fstab`

`:!sudo blkid /dev/webdata-vg/logs-lv`

![Updating-UUID](./images1/Update-UUID.PNG)

`code`
`code`
`code`
`code`
`code`
`code`
`code`
`code`
`code`
`code`
`code`
`code`
`code`
`code`
`code`
`code`