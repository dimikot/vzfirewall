vzfirewall: extremely simple tool which configures opened ports 
            and hosts for incoming connections in OpenVZ environment 
(C) dkLab, http://en.dklab.ru/lib/dklab_vzfirewall/


Vzfirewall tool allows you to open/close ports for incoming connections 
with no dependencies to foreign IP addresses. E.g. you may allow a hostname 
release.prod.example.com to connect to port 5432 of VE 1234 and leave all 
other ports closed by modifying 1234.conf file adding multiline FIREWALL 
directive into it - see SYNOPSIS below.

You must then run vzfirewall -a on your hardware node to apply changes 
made in *.conf.

Note that it is recommended to use hostnames instead of IP addresses here, 
so the configuration is persistent for VE movements to different IP-address: 
you just need to run vzfirewall -a again after movement. It is also 
reboot-safe, because applied to /etc/sysconfig/iptables (at RHEL systems).


INSTALLATION
------------

cd /usr/sbin
wget http://github.com/DmitryKoterov/vzfirewall/raw/master/vzfirewall
chmod +x vzfirewall


SYNOPSIS
--------

1. Modify the file /etc/sysconfig/vz-scripts/4.conf:
   ...
   FIREWALL="
       host.allowed.to.every.port
       yet.another.host
       * # means "any host"

       [25]
       host.allowed.to.access.smtp
       * # means "any"

       [80,443]
       hosts.allowed.to.access.two.ports
	
       [udp:53]
       *

       [CUSTOM]
       # You may use "$THIS" macro which is replaced by this machine IP
       # (and, if the machine has many IPs, it will be multiplicated).
       -A INPUT -i eth2 -d $THIS -j ACCEPT
       # Or you may use commands with no references to $THIS (only
       # such commands are allowed for 0.conf file).
       -A INPUT -i eth1 -j ACCEPT
   "
   ...
   We use FIREWALL directive in plain VE configs to allow to
   vzmigrate it easily from one node to another.

2. Run:
   # vzfirewall -a  - to apply rules
   # vafirewall -t  - to test rules with no application
