set firewall name DMZ-to-LAN default-action 'drop'  

set firewall name DMZ-to-LAN enable-default-log  

set firewall name DMZ-to-LAN rule 10 action 'accept'  

set firewall name DMZ-to-LAN rule 10 description 'wazuh aggent to wazuh server'  

set firewall name DMZ-to-LAN rule 10 destination address '172.16.200.10'  

set firewall name DMZ-to-LAN rule 10 destination port '1514,1515'  

set firewall name DMZ-to-LAN rule 10 protocol 'tcp'  

set firewall name DMZ-to-LAN rule 11 action 'accept'  

set firewall name DMZ-to-LAN rule 11 description 'allow established connections'  

set firewall name DMZ-to-LAN rule 11 state established 'enable'  

set firewall name DMZ-to-WAN default-action 'drop'  

set firewall name DMZ-to-WAN enable-default-log  

set firewall name DMZ-to-WAN rule 1 action 'accept'  

set firewall name DMZ-to-WAN rule 1 description 'accept established connections'  

set firewall name DMZ-to-WAN rule 1 state established 'enable'  

set firewall name DMZ-to-WAN rule 999 action 'accept'  

set firewall name DMZ-to-WAN rule 999 description 'allow outbound from nginx to WAN'  

set firewall name DMZ-to-WAN rule 999 disable  

set firewall name DMZ-to-WAN rule 999 source address '172.16.50.3'  

set firewall name LAN-to-DMZ default-action 'drop'  

set firewall name LAN-to-DMZ enable-default-log  

set firewall name LAN-to-DMZ rule 1 action 'accept'  

set firewall name LAN-to-DMZ rule 1 description 'allow wazuh connections back to DMZ'  

set firewall name LAN-to-DMZ rule 1 state established 'enable'  

set firewall name LAN-to-DMZ rule 2 action 'accept'  

set firewall name LAN-to-DMZ rule 2 description 'lan to web01'  

set firewall name LAN-to-DMZ rule 2 destination address '172.16.50.3'  

set firewall name LAN-to-DMZ rule 2 destination port '80'  

set firewall name LAN-to-DMZ rule 2 protocol 'tcp'  

set firewall name LAN-to-DMZ rule 2 source address '172.16.150.0/24'  

set firewall name LAN-to-DMZ rule 3 action 'accept'  

set firewall name LAN-to-DMZ rule 3 description 'allow SSH from mgmt01 to DMZ'  

set firewall name LAN-to-DMZ rule 3 destination address '172.16.50.0/29'  

set firewall name LAN-to-DMZ rule 3 destination port '22'  

set firewall name LAN-to-DMZ rule 3 protocol 'tcp'  

set firewall name LAN-to-DMZ rule 3 source address '172.16.150.10'  

set firewall name LAN-to-WAN default-action 'drop'  

set firewall name LAN-to-WAN enable-default-log  

set firewall name LAN-to-WAN rule 1 action 'accept'  

set firewall name WAN-to-DMZ default-action 'drop'  

set firewall name WAN-to-DMZ enable-default-log  

set firewall name WAN-to-DMZ rule 1 action 'accept'  

set firewall name WAN-to-DMZ rule 1 state established 'enable'  

set firewall name WAN-to-DMZ rule 1 state related 'enable'  

set firewall name WAN-to-DMZ rule 10 action 'accept'  

set firewall name WAN-to-DMZ rule 10 description 'allow http from WAN to DMZ'  

set firewall name WAN-to-DMZ rule 10 destination address '172.16.50.3'  

set firewall name WAN-to-DMZ rule 10 destination port '80'  

set firewall name WAN-to-DMZ rule 10 protocol 'tcp'  

set firewall name WAN-to-DMZ rule 22 action 'accept'  

set firewall name WAN-to-DMZ rule 22 description 'Allow ssh from WAN to jump on DMZ'  

set firewall name WAN-to-DMZ rule 22 destination address '172.16.50.4'  

set firewall name WAN-to-DMZ rule 22 destination port '22'  

set firewall name WAN-to-DMZ rule 22 protocol 'tcp'  

set firewall name WAN-to-LAN default-action 'drop'  

set firewall name WAN-to-LAN enable-default-log  

set firewall name WAN-to-LAN rule 1 action 'accept'  

set firewall name WAN-to-LAN rule 1 state established 'enable'  

set interfaces ethernet eth0 address '10.0.17.154/24'  

set interfaces ethernet eth0 description 'WAN'  

set interfaces ethernet eth1 address '172.16.50.2/29'  

set interfaces ethernet eth1 description 'DMZ'  

set interfaces ethernet eth2 address '172.16.150.2/24'  

set interfaces ethernet eth2 description 'LAN'  

set nat destination rule 22 description 'SSH to jump'  

set nat destination rule 22 destination port '22'  

set nat destination rule 22 inbound-interface 'eth0'  

set nat destination rule 22 protocol 'tcp'  

set nat destination rule 22 translation address '172.16.50.4'  

set nat destination rule 22 translation port '22'  

set nat destination rule 100 description 'nginx port forwarding'  

set nat destination rule 100 inbound-interface 'eth0'  

set nat destination rule 100 protocol 'tcp'  

set nat destination rule 100 translation address '172.16.50.3'  

set nat destination rule 100 translation port '80'  

set nat source rule 10 description 'NAT FROM DMZ TO WAN'  

set nat source rule 10 outbound-interface 'eth0'  

set nat source rule 10 source address '172.16.50.0/29'  

set nat source rule 10 translation address 'masquerade'  

set nat source rule 20 description 'NAT FROM DMZ TO LAN'  

set nat source rule 20 outbound-interface 'eth0'  

set nat source rule 20 source address '172.16.150.0/24'  

set nat source rule 20 translation address 'masquerade'  

set nat source rule 30 description 'NAT FROM MGMT TO WAN'  

set nat source rule 30 outbound-interface 'eth0'  

set nat source rule 30 source address '172.16.200.0/28'  

set nat source rule 30 translation address 'masquerade'  

set protocols rip interface eth2  

set protocols rip network '172.16.50.0/29'  

set protocols static route 0.0.0.0/0 next-hop 10.0.17.2  

set service dns forwarding allow-from '172.16.50.0/29'  

set service dns forwarding allow-from '172.16.150.0/24'  

set service dns forwarding listen-address '172.16.50.2'  

set service dns forwarding listen-address '172.16.150.2'  

set service dns forwarding listen-address '172.16.200.2'  

set service dns forwarding system  

set service ssh listen-address '0.0.0.0'  

set system host-name 'edge02-seraphim'  

set system name-server '10.0.17.2'  
