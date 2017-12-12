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


