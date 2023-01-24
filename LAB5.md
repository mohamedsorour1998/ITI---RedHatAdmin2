7- Use the fdisk command to create 2 Linux LVM (0x8e) partitions using "unpartitioned" space on
your hard disk. These partitions should all be the same size; to speed up the lab, do not make them
larger than 300 MB each. Make sure to write the changes to disk by using the w command to exit
the fdisk utility. Run the partprobe command after exiting the fdisk utility.
fdisk
n
+300M
0x8e

partprob /dev/HardName


8- Initialize your Linux LVM partitions as physical volumes with the pvcreate command. You can use
the pvdisplay command to verify that the partitions have been initialized as physical volumes.

pvcreate /dev/partition1 dev/partition2 dev/partition3
pvs === pvdisplay


9- Using only one of your physical volumes, create a volume group called test0. Use the vgdisplay
command to verify that the volume group was created.

vgcreate test0 /dev/partition1 dev/partition2 dev/partition3
vgdisplay


10- Create a small logical volume (LV) called data that uses about 30 percent of the available
space of the test0 volume group. Look for VG Size and Free PE/Size in the output of the
vgdisplay command to assist you with this. Use the lvdisplay command to verify your work.

lvcreate --size 90M --name data
lvdisplay

11- Create an ext2 filesystem on your new LV.
mkfs.ext2 /dev/test0/data


12- Make a new directory called /data and then mount the new LV under the /data directory.
Create a "large file" in this volume.
mkdir /data

mount /dev/test0/data /data 
ll -R / > /data/dumb


13- Enlarge the LV that you created in Sequence 1 (/dev/test0/data) by using approximately 25
percent of the remaining free space in the test0 volume group. Then, use the ext2online command
to enlarge the filesystem of the LV.
ext2online -d -v /dev/mapper/test0-data
lvextend -r -L +90M /dev/mapper/test0-data


14- Verify that the file /data/bigfile still exists in the LV. Run the df command and check to
verify that more free disk space is now available on the LV.
yes it is


15- Use the remaining extents in the test0 volume group to create a second LV called docs.

lvcreate --size 90M --name docs


16- Run the vgdisplay command to verify that there are no free extents left in the test0 volume
group.

vgdisplay


17- Create an ext2 filesystem on the new LV, make a mount point called /docs and mount the docs LV using this mount point.

mkfs.ext2 /dev/test0/docs
mkdir /docs
mount /dev/test0/docs /docs 



18- Add all of the remaining unused physical volumes that you created in Sequence 1 to the test0 volume group.
vgextend test0 /dev/partition3



19- If you run vgdisplay again, there now should be free extents (provided by the new physical
volumes) in the test0 volume group. Extend the docs LV and underlying filesystem to make use of
all of the free extents of the test0 volume group. Verify your actions.
Before moving on to the RAID sequence, disassemble your LVM-managed volumes by
taking the following actions:
Remove any /etc/fstab entries you created.
umount /dev/test0/data
lvremove /dev/test0/data
umount /dev/test0/docs
lvremove /dev/test0/docs
vgchange -an test0 (this deactivates the volume group)
vgremove test0 (this deletes the volume group)






20- Run the fdisk command and convert the Linux LVM (0x8e) partitions that were created in
above into Linux raid auto (0xfd) partitions. Save your changes and run the partprobe command

fdisk /dev/loop0
t
0x8e


21- Initialize your RAID array (RAID 0)
mdadm --create /dev/mdName --level=5 --raid=2 /dev/loop0/part1 /dev/loop0/part2


22- Format the RAID device with an ext3 filesystem
mkfs.ext3 /dev/mdName

23- Use the /data directory as a mount point for the /dev/md0 RAID device. Use the df
command to check the size of the filesystem.
mount /dev/mdName /data
