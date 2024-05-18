### Proxmox edit and reload network interfaces
```sh
nano /etc/network/interfaces
ifreload -a
```

### List network interfaces
```sh
ls -l /sys/class/net/
```
