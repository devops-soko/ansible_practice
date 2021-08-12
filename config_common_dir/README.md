## Title
config common dir by nfs and mount command


## Description
this playbook's functions
- make nfs dir under / in nodes and mount ansible server's target dir on /nfs 
- unmount and remove /nfs

## Environment
- Lab Structure
```
ansible server(192.168.74.100)               
    ├── node1(node_name : web1, ip: 192.168.74.101)
    └── node2(node_name : web2, ip: 192.168.74.102)
```

- OS & Package Version
    - Linux Version : CentOS 8.3.2011
    - Ansible Version : ansible 2.9.21


## Prerequisite
- install and config ssh & create ssh-key (need to do all servers)
```
# yum -y install openssh-server openssh-clients openssh-askpass
# sed -i 's/#Port 22/Port 22/g' /etc/ssh/sshd_config
# systemctl enable sshd
# systemctl start sshd

# firewall-cmd --permanent --zone=public --add-port=22/tcp
# firewall-cmd --reload
# ssh-keygen
```

- share public key with nodes (need to do only ansible server)
```
# ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.74.101
# ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.74.102
```

- install nfs on ansible server
```
# yum  -y install nfs-utils
```

- make dir to share
```
# mkdir /home/soko0305/nfs
# chmod 777 /home/soko0305/nfs
```

- change config that nodes can access the ansible server
```
# vi /etc/exports


add below commands
/home/soko0305/nfs 192.168.74.101(rw,sync)
/home/soko0305/nfs 192.168.74.102(rw,sync)
```

- restart & enable service
```
# systemctl restart rpcbind 
# systemctl restart nfs-server 
# systemctl enable rpcbind
# systemctl enable nfs-server
```

- confirm config
```
# systemctl status nfs-server
● nfs-server.service - NFS server and services
   Loaded: loaded (/usr/lib/systemd/system/nfs-server.service; enabled; vendor >
  Drop-In: /run/systemd/generator/nfs-server.service.d
           └─order-with-mounts.conf
   Active: active (exited) since Mon 2021-08-09 17:32:23 JST; 44s ago
  Process: 36896 ExecStopPost=/usr/sbin/exportfs -f (code=exited, status=0/SUCC>
  Process: 36894 ExecStopPost=/usr/sbin/exportfs -au (code=exited, status=0/SUC>
  Process: 36893 ExecStop=/usr/sbin/rpc.nfsd 0 (code=exited, status=0/SUCCESS)
  Process: 36918 ExecStart=/bin/sh -c if systemctl -q is-active gssproxy; then >
  Process: 36907 ExecStart=/usr/sbin/rpc.nfsd (code=exited, status=0/SUCCESS)
  Process: 36906 ExecStartPre=/usr/sbin/exportfs -r (code=exited, status=0/SUCC>
 Main PID: 36918 (code=exited, status=0/SUCCESS)

# lsof -i tcp:111
COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
systemd     1 root  108u  IPv4  41089      0t0  TCP *:sunrpc (LISTEN)
systemd     1 root  110u  IPv6  41091      0t0  TCP *:sunrpc (LISTEN)
rpcbind 36178  rpc    4u  IPv4  41089      0t0  TCP *:sunrpc (LISTEN)
rpcbind 36178  rpc    6u  IPv6  41091      0t0  TCP *:sunrpc (LISTEN)

# exportfs -v
/home/soko0305/nfs
                192.168.74.101(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)
/home/soko0305/nfs
                192.168.74.102(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)

```

- disable and check firewall & iptables
```
# systemctl disable firewalld.service
# systemctl disable iptables

# systemctl list-unit-files | grep fire
firewalld.service                                                      disabled
```


## Files Structure
- ansible
```
.
├── inventory               # Target hosts
└── playbook.yml            # The playbook to mount & unmount ansible server's dir
                            # on node's dir
```

- shared dir in ansible server
```
/home
└── soko0305                
    └── nfs                # Target dir 
```

- shared dir in nodes
```
/
└── nfs                 
```

## Usage
- mount
```
ansible-playbook playbook.yml -i inventory -u root -e file_path=192.168.74.100:/home/soko0305/nfs -t mount

```

- unmount
```
ansible-playbook playbook.yml -i inventory -u root -e file_path=192.168.74.100:/home/soko0305/nfs -t unmount

```

