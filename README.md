# RHEL Study Guide: Control the Process

[RHEL Study Guide - Table of Contents](https://github.com/pslucas0212/RHEL-Study-Guide) 

In this section we will look at techniques to reset the root password if lost and some basic troubleshooting techniques when the server isn't booting or booting with an error

We see the server is struggling to boot properly and the root password has been lost.  Let's change the root password.

First at boot option menu type any key but enter and navigate to the rescue boot option

[](/image/bootoption01.png)

Press the e key to enter the boot script.  In the boot script navigate to the line that start with...   Type Ctrl-e to go to the end of that line and type 'rd.break'  Press Ctrl-x to restart the server

[](/image/bootoption02.png)

Press Enter to enter maintenance

At the prompt type
```
sh-5.1# mount -o remount,rw /sysroot
sh-5.1# chroot /sysroot
```

Now we will change the root password
```
sh-5.1# passwd root
Changing password for user root
New password:
BAD PASSWORD: The password is shorter than 8 characters
Retype new password:
passwd: all authtencation tokens updated successfully
```

Configure the system to perform a full SELinux relabeling after booting.  Type exit twice to reboot the system
```
sh-5.1# touch /.autorelabel
exit
sh-5.1# exit
```

