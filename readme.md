# Roger-skyline-1.5

## V.2 VM Part

You have to run a Virtual Machine (VM) with the Linux OS of your choice (`Debian`, Jessie, CentOS 7...) in the hypervisor of your choice (VMWare Fusion, VirtualBox...).

- I Choose `debian` as OS.

• A disk size of 8 GB.

• Have at least one 4.2 GB partition.

• It will also have to be up to date as well as the whole packages installed to meet the demands of this subject.

## V.3 Network and Security Part

• You must create a non-root user to connect to the machine and work.

> It is created as non-root user as a default while It is installed.

• Use sudo, with this user, to be able to perform operation requiring special rights.

`sudo` is not a default command. So at this time using to access root `su` instead, and then install default packages that includes `sudo`

    su # which makes me install packages
    apt-get update -y && apt-get upgrade -y
    apt-get install sudo vim -y

After then, we need to add user, at this time `hnam` to sudo user. To do this, append `hnam ALL=(ALL:ALL) ALL` user to `etc/sudoers` file below `root ALL=(ALL:ALL) ALL`

Finally, when I typed `sudo` in command, It should work like this.

• We don’t want you to use the DHCP service of your machine. You’ve got to configure it to have a static IP and a Netmask in \30.

**Definintions**

- `DHCP`, Dynamic Host Configuration Protocol is a IP standard which makes host ip management simple
- `static IP` is a non-changing IP address, compared to `dynamic IP` which is constantly changing
- `Newmask` is a 32-bit "mask" used to divide an IP address into subnets and specify the network's available hosts.

So, in here, Instead of using DHCP, which keeps changing, change the setting to use a static IP

• You have to change the default port of the SSH service by the one of your choice. SSH access HAS TO be done with public keys. SSH root access SHOULD NOT be allowed directly, but with a user who can be root.

• You have to set the rules of your firewall on your server only with the services used outside the VM.

• You have to set a DOS (Denial Of Service Attack) protection on your open ports of your VM.

• You have to set a protection against scans on your VM’s open ports.

• Stop the services you don’t need for this project.

• Create a script that updates all the sources of package, then your packages and which logs the whole in a file named `/var/log/update_script.log`. Create a scheduled task for this script once a week at 4AM and every time the machine reboots.

• Make a script to monitor changes of the /etc/crontab file and sends an email to root if it has been modified. Create a scheduled script task every day at midnight.

## VI.1 Web Part

> You have to set a web server who should BE available on the VM’s IP or an host (`init.login.com` for exemple). About the packages of your web server, you can choose
between Nginx and Apache. You have to set a self-signed SSL on all of your services.
You have to set a web "application" from those choices:

• A login page.

• A display site.

• A wonderful website that blow our minds.

## VI.2 Deployment Part

Propose a functional solution for deployment automation.