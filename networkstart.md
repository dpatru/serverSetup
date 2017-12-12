## remote start the server from mac 

Some servers will wake up when they receive a magic packet. To send a magic packet from a mac: http://apple.stackexchange.com/questions/95246/wake-other-computers-from-mac-osx 

    mac$ brew install wakeonlan 
    mac$ wakeonlan -i 192.168.1.255 -p 7 E0:CB:4E:27:1B:F1 # note that ip addr is the subnetwork broadcast address and mac address is the address of the server (use ifconfig to find it.) 
    

