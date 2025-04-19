# Project 2 - Remote Access Research and Integration

---

## Table of Contents
1. [Firewall Configuration](#firewall-configuration)
2. [Server Configuration](#server-configuration)
3. [Client Configuration](#client-configuration)
4. [Testing & Troubleshooting](#testing--troubleshooting)

---

## Firewall Configuration

### 1. Allow WireGuard Traffic from WAN to Jump on DMZ

On `edge02` (your firewall/router):

```bash
set firewall name WAN-to-DMZ rule 500 action accept
set firewall name WAN-to-DMZ rule 500 description "Allow WG from WAN to jump on DMZ"
set firewall name WAN-to-DMZ rule 500 destination port 51820
set firewall name WAN-to-DMZ rule 500 protocol udp
```

### 2. Forward WireGuard Traffic to Jump

```bash
set nat destination rule 500 description 'WG to jump'
set nat destination rule 500 destination port '51820'
set nat destination rule 500 inbound-interface 'eth0'
set nat destination rule 500 protocol 'udp'
set nat destination rule 500 translation address '172.16.50.4'
set nat destination rule 500 translation port '51820'
```

### 3. Allow RDP from Jump on DMZ to mgmt02 (LAN)

```bash
set firewall name DMZ-to-LAN rule 20 action 'accept'
set firewall name DMZ-to-LAN rule 20 description 'Allow RDP from DMZ to LAN'
set firewall name DMZ-to-LAN rule 20 destination address '172.16.200.11'
set firewall name DMZ-to-LAN rule 20 destination port '3389'
set firewall name DMZ-to-LAN rule 20 protocol 'tcp'
set firewall name DMZ-to-LAN rule 20 source address '172.16.50.4'
```

---

## Server Configuration 
### 1. Install WireGuard

```bash
sudo apt update
sudo apt install wireguard -y
```

### 2. Create WireGuard Key Pairs and Enable IP Forwarding

```bash
sudo -i
cd /etc/wireguard/
umask 077; wg genkey | tee privatekey | wg pubkey > publickey
sysctl -w net.ipv4.ip_forward=1
```

### 3. Modify the `wg0.conf` Configuration

Create the configuration file `/etc/wireguard/wg0.conf` with the following content:

```ini
[Interface]
Address = 10.124.200.2/32
ListenPort = 51820
PrivateKey = <PASTE SERVER PRIVATE KEY>
SaveConfig = true
PreUp = iptables -t nat -A POSTROUTING -o ens160 -j MASQUERADE
PostDown = iptables -t nat -D POSTROUTING -o ens160 -j MASQUERADE

[Peer]
PublicKey = <PASTE CLIENT PUB KEY>
AllowedIPs = 10.124.200.100/32
Endpoint = 10.0.17.24:51820
```

### 4. Start the Server

```bash
wg-quick up wg0
```

To enable the server on boot:

```bash
systemctl enable --now wg-quick@wg0
```

---

## Client Configuration

### 1. Install WireGuard Client

Visit [WireGuard Install](https://www.wireguard.com/install) and select your operating system. Follow the installation steps.

### 2. Create Tunnel Configuration in the WireGuard GUI

1. Open the WireGuard GUI.
2. Add a new empty tunnel.
3. Copy the generated public key and send it to the server.
4. Paste the following configuration:

```ini
[Interface]
PrivateKey = <CLIENT PRIVATE KEY, GENERATED AUTOMATICALLY>
ListenPort = 51820
Address = 10.124.200.100/24

[Peer]
PublicKey = <PASTE SERVER PUB KEY>
AllowedIPs = 10.124.200.2/32, 172.16.200.11/32
Endpoint = 10.0.17.124:51820
PersistentKeepalive = 60
```

### 3. Activate the Tunnel

Click **Activate** in the WireGuard GUI. This will establish the connection to the server.

---

## Testing & Troubleshooting

### 1. Test Connection

To test the VPN:

- Ping the server's internal IP (e.g., `172.16.200.11`).
- Use RDP to connect to `mgmt02` via `172.16.200.11`.

### 2. Troubleshooting

- **Can't Ping the Server**: Ensure that the tunnel is up and the firewall rules are configured correctly.
- **RDP Error**: Ensure that `RDP` is enabled on `mgmt02` and that the firewall on `mgmt02` allows incoming RDP connections.
- **WireGuard Not Starting**: Check the WireGuard logs for errors with `journalctl -xe | grep wg`.

---

## Additional Notes

- Ensure that the WireGuard configuration is correct on both the server and client.
- Firewall rules on all relevant systems (jump box, mgmt02, etc.) must allow the appropriate traffic (WireGuard, RDP).
- Test connectivity with `ping` and `telnet` to ensure that the tunnel and RDP are working as expected.

---
