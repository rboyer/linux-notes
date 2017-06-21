LVM and LUKS
============

Unlock second hard drive using keyfile
----------

1. `sudo dd if=/dev/urandom of=/root/keyfile bs=1024 count=4`
2. `sudo chmod 0400 /root/keyfile`
3. `sudo cryptsetup luksAddkey /dev/sdX /root/keyfile`
4. edit `/etc/crypttab`
    * `sdX_crypt /dev/sdX /root/keyfile luks`
    * `sdX_crypt /dev/disk/by-uuid/UUID_GOES_HERE /root/keyfile luks`
5. edit `/etc/fstab`
    * `/dev/mapper/sdX_crypt /media/sdX ext4 defaults 0 2`
6. Because we edited `/etc/crypttab` we have to: `sudo update-initramfs -u -k all`
7. Validate `/etc/fstab`: `sudo mount -a`


References
----------

* LVM
  * https://linuxquestionhelp.blogspot.com/2013/05/how-to-manage-and-use-lvm-logical.html
* LUKS
  * https://www.cyberciti.biz/hardware/howto-linux-hard-disk-encryption-with-luks-cryptsetup-command/
  * https://www.howtoforge.com/automatically-unlock-luks-encrypted-drives-with-a-keyfile
  * https://unix.stackexchange.com/questions/64863/luks-storing-keyfile-in-encrypted-usb-drive
  * https://wiki.archlinux.org/index.php/Dm-crypt/Device_encryption
  * https://wiki.archlinux.org/index.php/Dm-crypt
  * https://askubuntu.com/questions/450895/mount-luks-encrypted-hard-drive-at-boot
  * http://lowtek.ca/roo/2012/how-to-add-second-drive-to-luks-ubuntu/
