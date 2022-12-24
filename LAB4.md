LAB4
1. Use fdisk -l to locate information about the partition sizes. \
![My Remote Image](https://user-images.githubusercontent.com/110028481/209425021-51226b64-70fe-41cf-a272-6b14aedbb896.png) \
2. Use fdisk to add a new logical partition that is 1GB in size. \
![My Remote Image](https://user-images.githubusercontent.com/110028481/209425034-e2d15839-f473-47aa-ba5b-58089916fac8.png) \
3. Did the kernel feel the changes? Display the content of /proc/partitions file? What did you notice? How to overcome that? \
Yes it felt it \
![My Remote Image](https://user-images.githubusercontent.com/110028481/209425042-37e7348a-1f98-4d54-8921-59f6e96b48ed.png) \
4. Make a new ext2 file system on the new logical partition you just created. \
![My Remote Image](https://user-images.githubusercontent.com/110028481/209425049-e9702361-2939-4c59-8132-4a3f80d3eae3.png) \
5. Create a directory, name it /data. \
sudo mkdir /data \
6. Add a label to the new filesystem, name it data. done
7. Add a new entry to /etc/fstab for the new filesystem using the label you just create. \
![My Remote Image](https://user-images.githubusercontent.com/110028481/209425060-f6958192-38d1-4601-8035-e622aad73319.png) \
8. Mount the new filesystem. \
![My Remote Image](https://user-images.githubusercontent.com/110028481/209425069-2120e87b-02d9-4883-9c05-0461760c9544.png) \
9. Display your swap size. \
![My Remote Image](https://user-images.githubusercontent.com/110028481/209425001-46b89eed-6339-4023-8ea1-9ae01dc35db2.png) \
10. Create a swap file of size 512MB. \
![My Remote Image](https://user-images.githubusercontent.com/110028481/209424995-e3422d4f-9068-4554-af08-75fec1f5ff1d.png) \
11. Add the swap file to the virtual memory of the system.
12. Display the swap size
11 &12 → \
![My Remote Image](https://user-images.githubusercontent.com/110028481/209424981-1df03d36-deb0-4a55-945c-a870dccf9511.png) \
13. Implement disk quotas for users on the /home directory by taking the following actions
a. Edit /etc/fstab and add the usrquota option to the /home filesystem
b. Remount the filesystem with the command mount -o remount /home
c. Use the quotacheck command to create the quota-tracking file
quotacheck /home
d. Use the quotaon command to enable quota tracking by the kernel quotaon /home
