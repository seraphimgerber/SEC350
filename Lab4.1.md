### Lab 4.1 - SEC-350

#### Here we go again. We are going to set up our fw01 box with some fun commands today!

Use these commands on fw01:\
set zone-policy zone WAN interface eth0\
set zone-policy zone DMZ interface eth1\
set zone-policy zone LAN interface eth2\
commit save

set firewall name WAN-to-DMZ default-action drop\
set firewall name DMZ-to-WAN default-action drop\
set firewall name WAN-to-DMZ enable-default-log\
set firewall name DMZ-to-WAN enable-default-log

set zone-policy zone WAN from DMZ firewall name DMZ-to-WAN\
set zone-policy zone DMZ from WAN firewall name WAN-to-DMZ\
commit\
save

Wow, that was a lot just for the first deliverable. The professor gave those to us, so the next ones are on our own. We have to figure out how to append rule 10 to the WAN-to-DMZ firewall. Good thing we know our way around vyos...\
Here are the commands:

configure\
set firewall name WAN-to-DMZ rule 10 action accept\
set firewall name WAN-to-DMZ rule 10 description "Allow HTTP from WAN to DMZ"\
set firewall name WAN-to-DMZ rule 10 destination address 172.16.50.3\
set firewall name WAN-to-DMZ rule 10 destination port 80\
set firewall name WAN-to-DMZ rule 10 protocol tcp\
commit\
save\
exit

My fw01 box almost crapped itself for the last few commands, but I think vcenter is having a lot of traffic right now. Anyway, we can check to connect via http and it will still fail. So, there's more to do!

The failure was due to another firewall, since the connection was stopped on the way back out, not the way in. So, we need to explicitly tell the DMZ-TO-WAN firewall to allow established connections initiated from the WAN back out again.

So we are gonna add a little bit more, now. Repeat after me:

