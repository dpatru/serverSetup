# Windows Fileserver Setup: how to setup samba on Ubuntu 16.04

## Install a new disk (optional)

Use the `gparted` command if you need to format the disk.

Find the name and UUID of the disk using the commands `lshw` and
`blkid /dev/sdb` (assuming your disk name is sdb).

Create a mount point:

    sudo mkdir /media/sambafiles

Prepare to mount the disk by adding the following line to your
fstab. (Replace the UUID in the example with yours that you found with
the `blkid` command: 

	UUID=1532e85b-0337-412d-9644-ef6ad282efdb /media/sambafiles vfat
    rw,uid=nobody,gid=nogroup,umask=0000 0 2

You can mount the disk like so:

    sudo mount -av

(If you need to unmount, use `sudo umount /media/sambafiles`.)

Test to make sure that you can write to the disk:

	sudo -u nobody touch /media/sambafiles/testfile

If you get a permission error, you've done something wrong. 

## Setup Samba

See
https://askubuntu.com/questions/781963/simple-samba-share-no-password
for more information.

If you want to have a clean install, at least one person recommends
purging the existing samba:

	sudo apt-get purge samba
	sudo rm -rf /etc/samba/ /etc/default/samba

Install samba if needed.

    sudo apt install samba

Configure samba by adding the following section to the end of the
samba config file (/etc/samba/smb.conf):

	[myfiles]
	path = /media/sambafiles
	browsable = yes
	force user = nobody
	force group = nogroup
	read only = no
	guest ok = yes
	create mask = 777

Run `testparm` when you're done to see if you've made typos.

Start or restart the samba deamon.

	sudo service smbd restart

Test that you can read and write the shared files. Test from both the
server computer and another computer on the network.

