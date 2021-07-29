## Title
check package and service status


## Description
There are 2 functions in this ansible role
- check if the package has been downloaded 
- check status of service


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
- install and config ssh
```
# yum -y install openssh-server openssh-clients openssh-askpass
# sed -i 's/#Port 22/Port 22/g' /etc/ssh/sshd_config
# systemctl enable sshd
# systemctl start sshd

# firewall-cmd --permanent --zone=public --add-port=22/tcp
# firewall-cmd --reload
```

- create ssh-key & share public key with nodes
```
# cd /root/.ssh/
# ssh-keygen -t rsa
# scp id_rsa.pub root@192.168.74.101:/root/.ssh/authorized_keys
# scp id_rsa.pub root@192.168.74.102:/root/.ssh/authorized_keys
```


## Files Structure
```
.
├── inventory               # Target hosts
└── playbook.yml            # The playbook to check package and service status
```

## Usage
- check package has been downloaded
```
ansible-playbook playbook.yml -i inventory -u root -t package_check  -e target_name=openssh
```
 
- check status of service
```
ansible-playbook playbook.yml -i inventory -u root -t service_check  -e target_name=firewalld
```
