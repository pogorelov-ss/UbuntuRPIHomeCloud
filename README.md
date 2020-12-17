# UbuntuRPIHomeCloud
howto configure ubuntu on rasberry pi 4 for home cloud .... vpn , nas , rdp, k3s


## install and init config

https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview

### change ip to static 

https://netplan.io/examples/

`ip a`

`sudo nano /etc/netplan/50-cloud-init.yaml`

change this:

```
network:
    ethernets:
        eth0:
            dhcp4: true
            match:
                driver: bcmgenet smsc95xx lan78xx
            optional: true
            set-name: eth0
    version: 2
```
to 

```

network:
    ethernets:
        eth0:
            dhcp4: no
            addresses: [192.168.88.25/24]
            gateway4: 192.168.88.1
            nameservers:
                addresses: [8.8.8.8, 1.1.1.1]
            match:
                driver: bcmgenet smsc95xx lan78xx
            set-name: eth0
    version: 2
```

`sudo netplan apply`

reconect with new IP

add pub key to the `.ssh/authorized_keys`

##  wireguard

`https://www.wireguard.com/install/`

### generate keys for WireGuard. One per node

`wg genkey | tee <filename>.key |  wg pubkey > <filename>-public.key`

### install

for ubuntu 20.04
`sudo apt update && sudo apt install wireguard -y`

### setup WireGuard on server and clients

`sudo nano /etc/wireguard/<interface name>.conf`

```
sudo nano /etc/wireguard/wg0.conf
```

server config
```
[Interface]
Address = 10.11.12.1/32
ListenPort = 51820
PrivateKey = <sever private key>
[Peer]
PublicKey = <client pub key>
AllowedIPs = 10.11.12.10/32
```

client config
```
[Interface]
Address = 10.11.12.10/32
PrivateKey = <client private key>
[Peer]
# server
PublicKey = <sever pub key>
AllowedIPs = 10.11.12.0/24
Endpoint = <external server IP>:51820
```
### startup WireGuard

`sudo wg-quick up wg0`

`sudo systemctl enable wg-quick@wg0.service`

## instatll xrdp

https://linuxize.com/post/how-to-install-xrdp-on-ubuntu-20-04/


