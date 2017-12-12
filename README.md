# serverSetup: a guide to setting up an ubuntu server

## ssh

### Install ssh
Install an ssh server. 

    server$ sudo apt-get-install openssh-server

### Enable ssh public key login

Create a local ssh key if one does not exist.

    local$ ls ~/.ssh/id_*.pub || ssh-keygen

Login and add a key for remote access. 
If you have ssh-copy-id on the local machine, use it. 

    local$ ssh-copy-id username@serverName.local

Otherwise, do it manually.

    local$ cat 
    local$ ssh username@serverName.local
    server-ssh-shell$ cd ~/
    server-ssh-shell$ mkdir -P ~/.ssh   # create a .ssh folder in your server's home dir
    server-ssh-shell$ chmod 700 ~/.ssh  # make the folder private.
    server-ssh-shell$ 
    local$ f=$(ls ~/.ssh/id_*.pub | head -n1)
    local$ cat $f | ssh username@serverName.local "cat - >> ~/.ssh/authorized_keys" # be sure to use >> (append) not > (overwrite)
    server-ssh-shell$ chmod 600 ~/.ssh/authorized_keys
    
Test the public key login:

    local$ ssh username@serverName.local # you should be able to login without a password
    server-ssh-shell$ sudo perl -piE 's/PermitRootLogin yes/PermitRootLogin no/; s/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
    server-ssh-shell$ service ssh restart

### ssh X forwarding
If you want to run X programs from ssh, use `ssh -X ...` or `ssh -Y ...`

    mac$ ssh -Y username@servername.local # X forwarding for mac

### ssh nicknames
You may want to create nicknames for servers in your ssh config file (`~/.ssh/config`).
Add an entry like the following:

    Host myservernickname
        HostName myservername.local
        User username
        IdentityFile ~/.ssh/id_rsa
        IdentitiesOnly yes

Now you can ssh into your server like this:

    local$ ssh myservernickname

## firewall

    local$ ssh username@serverName.local
    server-ssh-shell$ sudo apt-get install ufw
    # todo: open ssh port only

## safe mode

To start Ubuntu into safe mode (Recovery Mode) hold down the left Shift key as the computer starts to boot. If holding the Shift key doesn't display the menu press the Esc key repeatedly to display the GRUB 2 menu. From there you can choose the recovery option. On 12.10 the Tab key works for me.start server 

## turn off graphics

Disable lightdm: http://superuser.com/questions/1106174/boot-ubuntu-16-04-into-command-line-do-not-start-gui
    server$ sudo systemctl disable lightdm.service # This will prevent the service from starting at boot.
    
    server$ sudo systemctl start lightdm.service # Start up the window manager

See also: http://superuser.com/a/986918/208059
it is possible to disable a graphical boot completely. Copy

      cp /etc/default/grub /etc/default/grub-orig
The edit /etc/default/grub, comment out this line,

      #GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
modify this line to look like

      GRUB_CMDLINE_LINUX="text"
then uncomment this line,

      GRUB_TERMINAL=console
Save, run

       update-grub
when you reboot,if you do not have a broken installation, you will kind yourself in text mode. After reconfiguring X, youmay start the graphical session with

       startx


## remote start the server from mac

Some servers will wake up when they receive a magic packet. To send a magic packet from a mac: http://apple.stackexchange.com/questions/95246/wake-other-computers-from-mac-osx

    mac$ brew install wakeonlan
    mac$ wakeonlan -i 192.168.1.255 -p 7 E0:CB:4E:27:1B:F1 # note that ip addr is the subnetwork broadcast address and mac address is the address of the server (use ifconfig to find it.)
    

