### Proxmox Network
```sh
# Bridge: https://pve.proxmox.com/wiki/Network_Configuration
# WLAN: https://wiki.debian.org/WiFi/HowToUse
# 4Address: http://nullroute.lt/~grawity/journal-2011.html#post:20110826

# List network Interface
ls -l /sys/class/net/

# Change and apply config
nano /etc/network/interfaces
ifreload -a

# check is wlan connected
iw [network] link
```

### Proxmox VM
```sh
# Iso File Location
/var/lib/vz/template/iso

# get file size in MB
ls -l --block-size=M
```
