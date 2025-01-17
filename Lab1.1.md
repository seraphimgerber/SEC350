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
![{CA675A1E-8C7B-4859-80F7-5A3F38D908A3}](https://github.com/user-attachments/assets/335bdfc5-3528-464c-ba09-128e3e4799cf)
![{1C598D57-47B6-432C-B5FB-9CF4E63CCA9B}](https://github.com/user-attachments/assets/34406324-2c9d-4d26-9874-070400a2f2ce)
Then update using 'sudo netplan try'. If there are no errors, apply the settings!
![{07AED6FF-52CF-42EC-B8EC-F4B7EF7215FA}](https://github.com/user-attachments/assets/c6e05434-12a5-4557-badf-63106335a468)
Much better. Now we can connect to the internet!
Side note, some of my peers used 'nmtui' to configure their IP address. I mean, whatever is easier for you.
9. Remember to take a snapshot. SPLASH!

#### Now, let's set up fw01. Here's a checklist to follow:

1. Make sure all Network Adapters are connected. We will be adding a DMZ connection and a LAN connection on top of the WAN adapter.
2. Power on the box. This one takes a while to boot up... patience is a virtue.
3. Configure the hostname.
![{71880EA1-1FAC-4874-981B-CF4D58AA6493}](https://github.com/user-attachments/assets/01df1b35-17e4-43b7-b399-bd09aaac6361)
4. Configure the interfaces. We will use the following commands to edit in vyOS:
configure
set interfaces ethernet (interface) description (description)
and also
set interfaces ethernet (interface) address (IP address/mask)
commit
save
exit
![{0E3ED01A-F289-43E9-9179-A4FC674DB96F}](https://github.com/user-attachments/assets/c57e878d-7963-4c8f-a3d0-115e0d23ca90)
Looks good to me.
5. Configure gateways and DNS.
![{30C8984A-31A5-455D-9801-99B26FC97AB8}](https://github.com/user-attachments/assets/634c2493-2463-430c-9eee-08d9d1e98171)
Now we can ping google!
6. Remember to take a snapshot. SPLASH!

#### Next, we'll set up web01. Here's a checklist to follow:

1. Make sure the Network Adapter is connected. We wanna change it to the DMZ network.
2. Power on the box.
3. Set an IP address and hostname. NOW we will be using 'nmtui'
![{EB8806EF-7CE6-4B33-92C7-1190B6509F92}](https://github.com/user-attachments/assets/06f33860-a7be-4bc4-8870-2e94cc73bfd2)
![{A055DABE-9FE9-4A26-9731-88AA77E98EEF}](https://github.com/user-attachments/assets/2234c184-5078-40d6-bda4-cdaadbb8fac6)

