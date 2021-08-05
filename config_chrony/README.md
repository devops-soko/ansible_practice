## Title
config chrony


## Description
playbook process is like below
- install chrony package
- change chrony config file
- start chrony service
- add ntp firewall and reload 


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

- deploy chrony server
```
$ sudo yum install -y chrony
$ sudo vi /etc/chrony.conf   -> please change config file if you need (ex. change server, client ip range, etc.)
$ sudo systemctl enable chronyd
$ sudo systemctl start chronyd
$ sudo firewall-cmd --add-service=ntp --permanent
$ sudo firewall-cmd --reload
```

- confirm chorny service
```
$ systemctl status chronyd
$ timedatectl
$ chronyc sourcestats -v
```

## Files Structure
```
.
├── inventory               # Target hosts
└── playbook.yml            # The playbook to to config chrony
```

## Usage
```
$ ansible-playbook playbook.yml -i inventory -u root -e chrony_server={target_ip}

# example
$ ansible-playbook playbook.yml -i inventory -u root -e chrony_server=192.168.74.100

```
