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

Wow, that was a lot just for the first deliverable. The professor gave those to us, so the next ones are on our own. We have to figure out how to append rule 10 to the WAN-to-DMZ firewall. Good thing we know our way around vyos...

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

configure\
set firewall name DMZ-to-WAN rule 1 action accept\
set firewall name DMZ-to-WAN rule 1 description "Allow established HTTP back from DMZ to WAN"\
set firewall name DMZ-to-WAN rule 1 state established enable\
set firewall name DMZ-to-WAN default-action drop\
set firewall name DMZ-to-WAN enable-default-log\
set interfaces ethernet eth1 firewall out name DMZ-to-WAN\
commit\
set firewall name WAN-to-DMZ rule 20 action accept\
set firewall name WAN-to-DMZ rule 20 description "Allow DNS from WAN to DMZ"\
set firewall name WAN-to-DMZ rule 20 destination port 53\
set firewall name WAN-to-DMZ rule 20 protocol udp\
commit\
set protocols static route 172.16.50.0/29 next-hop 172.16.50.2\
commit\
save\
exit

Okay, now for the moment of truth... let's see if this works!

Okay, it didn't work. I've done a lot of troubleshooting, but I can't seem to figure it out. We are gonna come back to this later.

For deliverable 3, we have to set some more firewall and zone rules. Here you go:

set firewall name LAN-to-DMZ default-action drop\
set firewall name LAN-to-DMZ enable-default-log\
set firewall name DMZ-to-LAN default-action drop\
set firewall name DMZ-to-LAN enable-default-log\
commit\
save\
exit

Now the zones.

set zone-policy zone DMZ from LAN firewall name LAN-to-DMZ\
set zone-policy zone LAN from DMZ firewall name DMZ-to-LAN\
set zone-policy zone DMZ interface eth1\
set zone-policy zone LAN interface eth2\
commit\
save\
exit

Okay. Deliverable 4 now. We want to allow established wazuh traffic back:

configure\
set firewall name DMZ-to-LAN rule 10 action accept\
set firewall name DMZ-to-LAN rule 10 description "wazuh agent communication with wazuh server"\
set firewall name DMZ-to-LAN rule 10 destination address 172.16.200.10\
set firewall name DMZ-to-LAN rule 10 destination port 1514,1515\
set firewall name DMZ-to-LAN rule 10 protocol tcp\
commit\
save

Deliverable 5! LAN-to-WAN and WAN-to-LAN:

configure\
set firewall name LAN-to-WAN default-action drop\
set firewall name LAN-to-WAN enable-default-log\
set firewall name LAN-to-WAN rule 1 action accept\
set zone-policy zone LAN from WAN firewall name WAN-to-LAN\
set zone-policy zone LAN from LAN firewall name LAN-to-WAN\
commit\
save

set firewall name WAN-to-LAN default-action drop\
set firewall name WAN-to-LAN enable-default-log\
set firewall name WAN-to-LAN rule 1 action accept\
set firewall name WAN-to-LAN rule 1 state established enable\
set firewall name WAN-to-LAN rule 1 state related enable\
set zone-policy zone WAN from LAN firewall name LAN-to-WAN\
set zone-policy zone WAN from WAN firewall name WAN-to-LAN\
commit\
save

Deliverable 6! LAN to DMZ and DMZ to LAN. This one is gonna be a LOT!:

set firewall name LAN-to-DMZ default-action drop\
set firewall name LAN-to-DMZ enable-default-log\
set firewall name LAN-to-DMZ rule 10 action accept\
set firewall name LAN-to-DMZ rule 10 description "Allow HTTP from LAN to Web01"\
set firewall name LAN-to-DMZ rule 10 destination address 172.16.50.3\
set firewall name LAN-to-DMZ rule 10 destination port 80\
set firewall name LAN-to-DMZ rule 10 protocol tcp\
set firewall name LAN-to-DMZ rule 20 action accept\
set firewall name LAN-to-DMZ rule 20 description "Allow SSH from mgmt01 to DMZ"\
set firewall name LAN-to-DMZ rule 20 source address 172.15.150.10\
set firewall name LAN-to-DMZ rule 20 destination port 22\
set firewall name LAN-to-DMZ rule 20 protocol tcp\
commit\
save

set firewall name DMZ-to-LAN rule 1 action accept\
set firewall name DMZ-to-LAN rule 1 description "Allow established/related traffic"\
set firewall name DMZ-to-LAN rule 1 state established enable\
set firewall name DMZ-to-LAN rule 1 state related enable\
set zone-policy zone DMZ from DMZ firewall name DMZ-to-LAN\
commit\
save


