### Lab 1 - SEC-350

#### Here is what we are going to be configuring:

Rw01: This is the “road warrior” linux laptop.  A computer that sits outside your organization's network
Add sudo user
Configure IP configuration (ip, mask, gateway etc.)
Configure IP route to direct certain traffic to the organization’s DMZ

Fw01: This is a vyos router/firewall that connects the SEC-350 (ISP), DMZ, and LAN networks
Add and set adapters in VSphere
Configure hostname
Configure ip address configuration per the 3 interfaces
Set default routing rules
Set DNS forwarding and forwarding rules
Set NAT rules

Web01: This is the organization's CENTOS web server in the DMZ
Add user, set password, add to sudo (wheel) group
Set hostname
Set ip configuration (static) including Gateway and DNS
Set firewall rules
Configure as a web server
Configure as rsyslog client

Log01: This is the organization’s CentOS log server (in DMZ for now)
Add user, set password, add to sudo (wheel) group
Set hostname
Set ip configuration (static) including Gateway and DNS
Set firewall rules
Configure as rsyslog server

#### Let's start with configuring rw01. Here's a checklist to follow:

1. Make sure the Network Adapter is connected.
2. Power on the box.
3. Change the password to secure the user. Use command 'passwd'
4. Add a user. Use command 'adduser seraphim' 
5. Give the user privileges. Use command 'sudo usermod -aG sudo seraphim'
6. Set a hostname. Use command 'sudo hostnamectl set-hostname rw01-seraphim'
7. Double check that the static IP address matches the assigned IP using 'ip a'

![{60D370A3-724B-4778-9603-5F1B1B8A9B4D}](https://github.com/user-attachments/assets/b6c5e5e4-3719-4613-8d15-181d31ee07da)

As you can see, there's no IP address assigned. Here's how to fix that:

![{51869229-C30E-42AD-954C-270AB8248293}](https://github.com/user-attachments/assets/7c28690e-8007-4211-a15d-675deb21f989)

Then update using 'sudo netplan try'. If there are no errors, apply the settings!

![{07AED6FF-52CF-42EC-B8EC-F4B7EF7215FA}](https://github.com/user-attachments/assets/c6e05434-12a5-4557-badf-63106335a468)

Much better. Now we can connect to the internet!
Side note, some of my peers used 'nmtui' to configure their IP address. I mean, whatever is easier for you.

Also, I had issues configuring the routes using the GUI, so I added the routes in the network configuration file. 

9. Remember to take a snapshot. SPLASH!

#### Now, let's set up fw01. Here's a checklist to follow:

1. Make sure all Network Adapters are connected. We will be adding a DMZ connection and a LAN connection on top of the WAN adapter.
2. Power on the box. This one takes a while to boot up... patience is a virtue.
3. Configure the hostname.

![{71880EA1-1FAC-4874-981B-CF4D58AA6493}](https://github.com/user-attachments/assets/01df1b35-17e4-43b7-b399-bd09aaac6361)

4. Configure the interfaces. We will use the following commands to edit in vyOS:
configure
set interfaces ethernet (interface) description (description)
set interfaces ethernet (interface) address (IP address/mask)
commit
save
exit

![{0E3ED01A-F289-43E9-9179-A4FC674DB96F}](https://github.com/user-attachments/assets/c57e878d-7963-4c8f-a3d0-115e0d23ca90)

Looks good to me.

5. Configure gateways and DNS.

![{30C8984A-31A5-455D-9801-99B26FC97AB8}](https://github.com/user-attachments/assets/634c2493-2463-430c-9eee-08d9d1e98171)

Now we can ping google!

6. Now we configure for NAT forwarding.

configure

set nat source rule 10 description "NAT FROM DMZ to WAN"

set nat source rule 10 outbound-interface eth0

set nat source rule 10 source address 172.16.50.0/29

set nat source rule 10 translation address masquerade

commit

save

7. Now configure for DNS forwarding.

configure

set service dns forwarding listen-address 172.16.50.2

set service dns forwarding all-from 172.16.50.0/19

set service dns forwarding system

commit

save

8. Remember to take a snapshot. SPLASH!

#### Next, we'll set up web01. Here's a checklist to follow:

1. Make sure the Network Adapter is connected. We wanna change it to the DMZ network.
2. Power on the box.
3. Set an IP address and hostname. NOW we will be using 'nmtui'

![{EB8806EF-7CE6-4B33-92C7-1190B6509F92}](https://github.com/user-attachments/assets/06f33860-a7be-4bc4-8870-2e94cc73bfd2)
![{A055DABE-9FE9-4A26-9731-88AA77E98EEF}](https://github.com/user-attachments/assets/2234c184-5078-40d6-bda4-cdaadbb8fac6)

4. Add a user and set a password. Use commands 'adduser' and 'passwd'
5. Give the user sudo privileges using 'sudo usermod -aG wheel'
6. Now we will configure httpd. First, we need to install it using 'sudo yum install httpd -y'
   
![{41E5CCB1-2B6F-4072-AA87-C81565C062DF}](https://github.com/user-attachments/assets/818a2bbd-b864-4006-9d04-1a434d454146)

Looking good!

9. Now we will add the ports 80 and 443 for our server.

![{7122EABB-CAAB-43BC-B933-5A874F30C640}](https://github.com/user-attachments/assets/83a13da5-11e0-4868-abdd-e918ed94b678)

10. Remember to take a snapshot. SPLASH!

#### Now we will set up log01. Here's a checklist to follow:
1. Make sure the proper Network Adapter is connected.
2. Set up the IP address and hostname using 'nmtui'
3. Add a user and set a password. Use commands 'adduser' and 'passwd'
4. Give the user sudo privileges using 'sudo usermod -aG wheel'
5. Enable the ports for rsyslog using 'sudo firewall-cmd --zone=public --permanent --add-port=514/tcp' and do the same for udp
6. Take a snapshot. SPLASH!

