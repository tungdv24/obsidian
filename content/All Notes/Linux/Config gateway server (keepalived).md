# Config gateway server (keepalived)
28-03-2025
Tags: #linux 


- Nối hai đường mạng vào trên một con server
- 99-netcfg-vmware.yaml (Cho đường mạng private)

```bash
network:
  version: 2
  renderer: networkd
  ethernets:
    ens160:
      dhcp4: no
      dhcp6: no
      addresses:
        - 192.168.10.170/24
      routes:
        - to: 192.168.10.0/24
          via: 192.168.10.170
      nameservers:
        addresses:
          - 8.8.8.8
```

- 98-netcfg-vmware.yaml (Cho đường mạng public)

```bash
network:
  version: 2
  renderer: networkd
  ethernets:
    ens192:
      dhcp4: no
      dhcp6: no
      addresses:
        - 192.168.10.197/24
      routes:
        - to: default
          via: 192.168.10.1
      nameservers:
        addresses:
          - 8.8.8.8
```

- Config port forwarding

```bash
sudo nano /etc/sysctl.conf
```

```bash
net.ipv4.conf.default.forwarding=1
net.ipv4.conf.all.forwarding=1
```

```bash
sudo sysctl -p
net.ipv4.conf.default.forwarding = 1 #Output
net.ipv4.conf.all.forwarding = 1
```

```bash
sudo iptables -t nat -A POSTROUTING -o ens192 -j MASQUERADE
sudo iptables -A FORWARD -i ens160 -o ens192 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i ens192 -o ens160 -j ACCEPT
```

```bash
  sudo apt install iptables-persistent
  sudo netfilter-persistent save
  sudo netfilter-persistent reload
```

### Config keepalived cho 2 gateway

```bash
sudo apt install keepalived
```

```bash
nano /etc/keepalived/keepalived.conf
```

- Server 1 (master)

```bash
global_defs {
    # set hostname
    router_id gate01
}

vrrp_instance VRRP1 {
    # on primary node, specify [MASTER]
    # on backup node, specify [BACKUP]
    # if specified [BACKUP] + [nopreempt] on all nodes, automatic failback is disabled
    state MASTER
    # if you like disable automatic failback, set this value with [BACKUP]
    # nopreempt
    # network interface that virtual IP address is assigned
    interface ens160
    # set unique ID on each VRRP interface
    # on the a VRRP interface, set the same ID on all nodes
    virtual_router_id 101
    # set priority : [Master] > [BACKUP]
    priority 200
    # VRRP advertisement interval (sec)
    advert_int 1
    # virtual IP address
    virtual_ipaddress {
        192.168.10.189/24
    }
}

```

- Server 2 (backup)

```bash
global_defs {
    router_id gate02
}

vrrp_instance VRRP1 {
    state BACKUP
    # nopreempt
    interface ens160
    virtual_router_id 101
    priority 100
    advert_int 1
    virtual_ipaddress {
        192.168.10.189/24
    }
}
```

```bash
systemctl restart keepalived
```

# References