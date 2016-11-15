# serverSetup: a guide to setting up an ubuntu server

## ssh

Install an ssh server. 

    server$ sudo apt-get-install openssh-server

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
    local$ ssh username@serverName.local # you should be able to login without a password
    server-ssh-shell$ chmod 600 ~/.ssh/authorized_keys
    server-ssh-shell$ sudo perl -piE 's/PermitRootLogin yes/PermitRootLogin no/; s/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
    server-ssh-shell$ service ssh restart
    

## firewall

    local$ ssh username@serverName.local
    server-ssh-shell$ sudo apt-get install ufw
    # todo: open ssh port only
