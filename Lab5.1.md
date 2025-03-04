### Lab 5.1 - SEC-350

#### Today's menu is Wazuh WAF.

First step, configure fw01 so that established connections from the DMZ-to-WAN are allowed back through the WAN-to-DMZ firewall.

set firewall name WAN-to-DMZ rule 1 action accept
set firewall name WAN-to-DMZ rule 1 state established enable

set firewall name DMZ-to-WAN rule 999 action accept
set firewall name DMZ-to-WAN rule 999 source address 172.16.50.3

Rule 999 is temporary!!! We will get rid of it once we have downloaded mod_security.

Now on web01, we add mod_security:

sudo yum install mod_security mod_security_crs php php-common php-opcache php-cli php-gd php-curl php-mysqlnd -y
