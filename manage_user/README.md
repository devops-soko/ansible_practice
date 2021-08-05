## Title
Manage linux users by ansible


## Description
There are 3 functions in this ansible role
- add user
- remove user
- debug


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


## Files Structure
```
.
├── inventory               # Target hosts
├── playbook.yml            # The playbook which is going to execute role
└── roles                 
    └── manage_user       
        ├── defaults
        │   └── main.yml
        ├── files
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── README.md
        ├── tasks         # add user, remove user, debug tasks are defined in here
        │   └── main.yml
        ├── templates
        ├── tests
        │   ├── inventory
        │   └── test.yml
        └── vars          # Variable = type : list[dic{}, dic{}] / userid, password
            └── main.yml
```


## Usage
- add user
```
$ ansible-playbook ./playbook.yml -i ./inventory -u root -t adduser
```
 
- remove user
```
$ ansible-playbook ./playbook.yml -i ./inventory -u root -t rmuser
```
