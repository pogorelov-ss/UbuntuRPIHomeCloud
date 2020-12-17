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

