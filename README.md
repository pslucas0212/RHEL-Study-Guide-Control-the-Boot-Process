# RHEL Study Guide: Control the Process

[RHEL Study Guide - Table of Contents](https://github.com/pslucas0212/RHEL-Study-Guide) 

In this section we will look at techniques to reset the root password if lost and some basic troubleshooting techniques when the server isn't booting or booting with an error

We see the server is struggling to boot properly and the root password has been lost.  Let's change the root password.

First at the boot option menu type any key but enter and navigate to the rescue boot option

![](/img/bootoption01.png)

Press the e key to enter the boot script.  In the boot script navigate to the line that starts with linux...   Type Ctrl-e to go to the end of that line and type 'rd.break'  Press Ctrl-x to restart the server

![](/img/bootoption02.png)

Press Enter to enter maintenance
![](/img/bootoption03.png)

At the prompt type
![](/img/bootoption04.png)
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

At boot option menu type any key but enter and navigate to the "normal" boot option

![](/img/bootoption05.png)

Press the e key to enter the boot script.  In the boot script navigate to the line that starts with linux...   Type Ctrl-e to go to the end of that line and type 'systemd.unit=emergency.target'  Press Ctrl-x to restart the server

![](/img/bootoption06.png)

At the next prompt type the root password we create above

![](/img/bootoption07.png)

At the prompt type..
```
[root@serverb~]# mount -o remount,rw /
```
Mount all volumes and we will see a volume error
```
[root@serverb~]# mount -a
mount: /olddata: can't find UUID=4a5....0bc.
```
Edit /etc/fstab and comment out the problem line
```
# vi /etc/fstab
```

Reload the systemd daemon, and make sure we can mount the volumes without an error.  Finally reboot the server
```
[root@serverb~]# systemctl daemon-reload
[root@serverb~]# mount -a
[root@serverb~]# systemctl reboot
```

The system should now boot successfully and you will see a serverb login prompt.  Lets change the systemd target.  ssh to serverb, sudo -i and let's see what target is set.
```
# systemctl get-default
multi-user.target
```

Let's change the default.target to graphical.target
```
# systemctl set-default graphical.target
# systemctl get-default
graphical.target
```

We are finished
