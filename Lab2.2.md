### Lab 2.2 - SEC-350

#### Today's mission: Syslog Organization on log01!!!

With all my cyber classes, I keep forgetting which is which in terms of labs and VMs. But for this one, I changed all the usernames and passwords from the default. This is a reminder to myself.

So to start off, we are going to set up mgmt01 like we did for the other VMs earlier. This one is xubuntu. we also have to access it using Chrome Remote Desktop. You know the drill...
1. Change the network adapter to LAN.

![image](https://github.com/user-attachments/assets/e5361029-7fdf-4e18-a7f1-908c7a5e3310)

2. Now we add the appropriate IP address. sudo nano /etc/netplan/01-netcfg.yaml

![image](https://github.com/user-attachments/assets/8ed01d89-36fc-46ab-99d6-5dd77195a870)

3. sudo netplan try

![image](https://github.com/user-attachments/assets/a3fff228-908a-4569-a2e9-b2332fc5b30d)

Yay! It works.
4. Change the default password and add an administrative user.

![image](https://github.com/user-attachments/assets/652d9679-cfbe-44b8-b04a-d4636f36e832)

All that good stuff.
5. Now we have to add a NAT source rule for DMZ. I should've done this earlier. Go to fw01 and type these:
configure

set nat source rule 110 outbound-interface eth0

set nat source rule 110 source address 172.16.150.0/24

set nat source rule 110 translation address masquerade

commit

save

configure

set service dns forwarding listen-address 172.16.150.2

set service dns forwarding allow-from 172.16.150.0/24

commit

save

6. Finally, we need to install remote desktop. First, open Chrome and log in. Download the Windows Chrome Remote Desktop. Then, we have to install it on our log01 box.

 
