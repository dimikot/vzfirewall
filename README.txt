vzfirewall: extremely simple tool which configures opened ports 
            and hosts for incoming connections in OpenVZ environment 
(C) dkLab, http://dklab.ru/lib/dklab_vzfirewall/


SYNOPSYS
--------

File /etc/sysconfig/vz-scripts/4.conf, FIREWALL directive:

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
