# Roger-skyline-1.5

## V.2 VM Part

You have to run a Virtual Machine (VM) with the Linux OS of your choice (`Debian`, Jessie, CentOS 7...) in the hypervisor of your choice (VMWare Fusion, VirtualBox...).

- I Choose `debian` as OS.

- A disk size of 8 GB, and have at least one 4.2 GB partition.

![Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_12.18.45_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_12.18.45_PM.png)

make a partition in `/`(root) directory as `primary` with 4.2GB, and `sgoinfre` as `logical` drive with 4.4GB.

- It will also have to be up to date as well as the whole packages installed to meet the demands of this subject.

## V.3 Network and Security Part

- You must create a non-root user to connect to the machine and work.

> It is created as non-root user as a default while It is installed.

### Use sudo, with this user, to be able to perform operation requiring special rights.

![Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_1.18.57_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_1.18.57_PM.png)

`sudo` is not a default command. So at this time using to access root `su` instead, and then install default packages that includes `sudo`

    su # which makes me install packages
    apt-get update -y && apt-get upgrade -y
    apt-get install sudo vim -y

After then, we need to add user, at this time `hnam` to sudo user. To do this, append `hnam ALL=(ALL:ALL) ALL` user to `etc/sudoers` file below `root ALL=(ALL:ALL) ALL`

Before editting the file, should change the mode as writeable with command `chmod 600 sudoers`. And finishing the file, with `source` command, make it work.

Finally, when I typed `sudo` in command, It should work like this.

![Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_1.29.24_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_1.29.24_PM.png)

### We don’t want you to use the DHCP service of your machine. You’ve got to configure it to have a static IP and a Netmask in \30.

**Definintions**

- `DHCP`, Dynamic Host Configuration Protocol is a IP standard which makes host ip management simple
- `static IP` is a non-changing IP address, compared to `dynamic IP` which is constantly changing
- `Newmask` is a 32-bit "mask" used to divide an IP address into subnets and specify the network's available hosts.

So, in here, Instead of using DHCP, which keeps changing, change the setting to use a static IP

1. create a file inside `/etc/network/interfaces.d` named `enp0s3`

        iface enp0s3 inet static
        			address 10.0.2.15
        			netmask 255.255.255.252

2. restart the network with `$ sudo service networking restart`
3. now `ifconfig` shows as I set up.

![Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_2.30.17_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_2.30.17_PM.png)

### appendix

- [how to setup a static ip address on debian](https://linuxconfig.org/how-to-setup-a-static-ip-address-on-debian-linux)
- [the way handle static IP in virtual Box](https://www.codesandnotes.be/2018/10/16/network-of-virtualbox-instances-with-static-ip-addresses-and-internet-access/)

### You have to change the default port of the SSH service by the one of your choice. SSH access HAS TO be done with public keys. SSH root access SHOULD NOT be allowed directly, but with a user who can be root.

To change a default port for ssh, need to update `/etc/network/sshd_config` file, which makes port number not commented, also change to something I want. As for me, I set with `54242`

![Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_2.44.44_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_2.44.44_PM.png)

then, restart the ssh server with `sudo service sshd restart` command, and check if it changed or not with `sudo systemctl status ssh` command. The below result shows the port has been changed to 54242.

![Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_2.46.26_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_2.46.26_PM.png)

finally, check It can access via ssh command with `username@static_ip -p PORT`

![Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_2.45.44_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-26_at_2.45.44_PM.png)

### You have to set the rules of your firewall on your server only with the services used outside the VM.

**[UFW (Uncomplicated Firewall)](https://help.ubuntu.com/community/UFW)** is a the default firewall configuration tool for Ubuntu is ufw. Developed to ease iptables firewall configuration, ufw provides a user friendly way to create an IPv4 or IPv6 host-based firewall. By default UFW is disabled.

    sudo apt-get install ufw
    sudo ufw status # to check if it is working or not
    sudo ufw enable # make it work
    
    # how to use
    sudo ufw allow [service name] 

![Roger%20skyline%201%205/Screen_Shot_2020-02-28_at_1.50.30_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-28_at_1.50.30_PM.png)

using `alllow` and `deny` command with ufw, we can make a firewall for each services

### You have to set a DOS (Denial Of Service Attack) protection on your open ports of your VM.

> **[fail2ban](https://doc.ubuntu-fr.org/fail2ban)** is an application that analyzes the logs of various services (SSH, Apache, FTP, etc.) by looking for correspondences between patterns defined in its filters and the log entries. When a match is found one or more actions are performed. Typically, fail2ban looks for repeated attempts of unsuccessful connections in the log files and proceeds to a ban by adding a rule to the iptables or nftables firewall to ban the source IP address.

![Roger%20skyline%201%205/Screen_Shot_2020-02-28_at_2.28.58_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-28_at_2.28.58_PM.png)

so, we need to copy `Fail2ban.conf` and `jail.conf` file as `jail.hnam.conf` not to be messed up.

To protect ssh servers, below `JAILS` add this information in the `jail.hnam.conf`

![Roger%20skyline%201%205/Screen_Shot_2020-02-28_at_3.14.35_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-28_at_3.14.35_PM.png)

also to protect a http server, at the bottom line of file, implements `[http-get-dos` specifies.

![Roger%20skyline%201%205/Screen_Shot_2020-02-28_at_3.17.07_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-28_at_3.17.07_PM.png)

finally, to make `fail2ban` filter thoes services, add or change a file inside `filter.d` folder. ssh is set up by default, we need to add `http-get-dos.conf` 

![Roger%20skyline%201%205/Screen_Shot_2020-02-28_at_3.22.04_PM.png](Roger%20skyline%201%205/Screen_Shot_2020-02-28_at_3.22.04_PM.png)

All settings are done, to be able to activate,

    sudo ufw reload
    sudo service fail2ban restart

Then, we can check status with `fail2ban-client status` command.

![Roger%20skyline%201%205/fail2ban_check.png](Roger%20skyline%201%205/fail2ban_check.png)

• You have to set a protection against scans on your VM’s open ports.

• Stop the services you don’t need for this project.

• Create a script that updates all the sources of package, then your packages and which logs the whole in a file named `/var/log/update_script.log`. Create a scheduled task for this script once a week at 4AM and every time the machine reboots.

• Make a script to monitor changes of the /etc/crontab file and sends an email to root if it has been modified. Create a scheduled script task every day at midnight.

## VI.1 Web Part

> You have to set a web server who should BE available on the VM’s IP or an host (`init.login.com` for exemple). About the packages of your web server, you can choose between Nginx and Apache. You have to set a self-signed SSL on all of your services. You have to set a web "application" from those choices:

• A login page.

• A display site.

• A wonderful website that blow our minds.

## VI.2 Deployment Part

Propose a functional solution for deployment automation.

The default firewall configuration tool for Ubuntu is

ufw

. Developed to ease

iptables

firewall configuration,

ufw

provides a user friendly way to create an IPv4 or IPv6 host-based firewall. By default UFW is disabled.