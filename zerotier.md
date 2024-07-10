Mirror from: https://www.reddit.com/r/Proxmox/comments/jctd6x/comment/g93vloi/?utm_medium=android_app&utm_source=share&context=3

Instructions for Zerotier exit Gateway in a Debian 10 lxc Container
1. Create Debian 10 container and put it on the same vmbr as the network you want to reach
2. Add this to your Container Config: lxc.mount.entry: /dev/net dev/net none bind,create=dir (config at: /etc/pve/lxc/<container-id>.conf)
3. Enable ip forwarding by editing /etc/sysctl.conf and uncommenting net.ipv4.ip_forward=1
4. Run sysctl -p to apply ip forwarding (it should read the line back to you)
5. install curl pgp iptables and iptables-persistent with apt update && apt install curl pgp iptables iptables-persistent
6. install zerotier with curl -s https://install.zerotier.com | bash
7. join zerotier network with zerotier-cli join <networkid>
8. Accept client in zerotier central
9. Add a Route to the Local Network you want to reach via zerotier in zerotier central (set the "Destination" Field to your local network address for example 192.168.1.0/24 and set the "Via" Field to the Zerotier ipaddress of the LXC Container)
10. Edit The File /etc/iptables/rules.v4 and paste the following:
```
*nat
:PREROUTING ACCEPT [0:0]
:INPUT ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
-A POSTROUTING -o eth0 -s 192.168.193.0/24 -j SNAT --to-source 192.168.1.1
COMMIT
*filter
:INPUT ACCEPT [0:0]
:FORWARD DROP [0:0]
-A FORWARD -i zt+ -s 192.168.193.0/24 -d 0.0.0.0/0 -j ACCEPT
-A FORWARD -i eth0 -s 0.0.0.0/0 -d 192.168.193.0/0 -j ACCEPT
:OUTPUT ACCEPT [0:0]
COMMIT
```
11. Replace all instances of 192.168.193.0/24 with your Zerotier network.
12. Replace 192.168.1.1 with the ipaddress of the lxc container in your local network
13. Run iptables-restore < /etc/iptables/rules.v4

You should now be able to reach the clients in your local network from your Zerotier network. If you have any question just ask.
