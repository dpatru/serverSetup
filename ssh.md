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

