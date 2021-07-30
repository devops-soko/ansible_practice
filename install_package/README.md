## Title
install & remove package and start & stop service 


## Description
There are 4 functions in this ansible role
- install package 
- remove package 
- start & enable service
- stop & diable service  


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
└── playbook.yml            # The playbook to install & remove package and to start & stop service 

```

## Usage
- install package 
```
$ ansible-playbook playbook.yml -i inventory -u root -t install_package  -e package_name=httpd

```
- remove package 
```
$ ansible-playbook playbook.yml -i inventory -u root -t remove_package  -e package_name=httpd

```
- start & enable service
```
$ ansible-playbook playbook.yml -i inventory -u root -t start_service  -e package_name=httpd

```
- stop & diable service  
```
$ ansible-playbook playbook.yml -i inventory -u root -t stop_service  -e package_name=httpd

```
