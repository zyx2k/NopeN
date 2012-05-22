NopeN Cookbook

/#
This scripts were created by Campbell Murray  <@zyx2k>
When executed they will reply to any TCP or UDP request on any port making all ports appear open.

It is available for public use, modification and redistribution under the GPL license
agreement.  

NOTES: This script was tested as working on an Ubuntu 11.04 32 bit server instance
running in VirtualBox on a Fedora 16 KDE operating system.  The VM has a 4GB drive and
1 GB of RAM.  The server was installed with a GB keyboard and locale.  Apparmour and ufw
were removed and ruby1.8 installed.

This is version 0.1 for public release.
#/

It is my intention to release a VM image for NopeN soon, however Beta testing has been fraught with problems while importing the VM to a new host.  So, I have decided to make available the basic scripts that configure a Linux server to answer all ports, UDP and TCP, as open.

The basic scripts are written in Ruby and require IP Tables to be installed.

Save the scripts as /usr/local/bin/NopeNTCP and /usr/local/bin/NopeNUDP on your Linux system, chmod +x and execute them to answer all TCP ports as open:

Both scripts can be run at the same time. If you want to keep a port live, e.g. SSH then you need to place an IP Table rule above the catchalls.

In order to make the changes permanent, should you wish them to be, follow these steps.

Firstly remove or comment out the iptable commands in the scripts, then set the iptables by running the following as root:

iptables -t nat -A PREROUTING -m tcp -p tcp -j REDIRECT --to-port 1
iptables -t nat -A PREROUTING -m udp -p udp -j REDIRECT --to-port 2

Save the iptables configuration for loading at boot i.e. make permanent

iptables-save > /etc/nopen.rules

Create file the file runNopeN in /etc/init.d/ and set chmod +x
Add these lines into the file

#!/bin/sh
iptables-restore /etc/nopen.rules
/usr/local/bin/NopeNUDP &
/usr/local/bin/NopeNTCP &

Next execute:

update-rc.d runNopeN defaults

Now, on every boot the iptables from the nopen.rules file will be loaded and the two scripts, NopeNUDP and NopeNTCP will be loaded.
