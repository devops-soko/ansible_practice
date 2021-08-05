## Title
config node to ssh without password 


## Description
step 1) check if there is .ssh dicrectory & authorized_key file

step 2) if there is no .ssh dicrectory -> genterate .ssh dicrectory

step 3) share ssh public key to nodes' home directory

step 4) if there is no authorized_key file -> copy shared public key to .ssh as authorized_key file & chagned permissions as 600


step 5) if there is authorized_key file -> append shared public key to authorized_key 
file

step 6) remove public key in user's home directory


## Environment
- Lab Structure
```
ansible server(192.168.74.100)               
    ├── node1(node_name : web1, ip: 192.168.74.101) -> username : user1, password : ***
    └── node2(node_name : web2, ip: 192.168.74.102) -> username : user1, password : ***
```
cf. user needs to use same username and password in each nodes

- OS & Package Version
    - Linux Version : CentOS 8.3.2011
    - Ansible Version : ansible 2.9.21


## Prerequisite
- install and config ssh (need to do all servers)
```
# yum -y install openssh-server openssh-clients openssh-askpass
# sed -i 's/#Port 22/Port 22/g' /etc/ssh/sshd_config
# systemctl enable sshd
# systemctl start sshd

# firewall-cmd --permanent --zone=public --add-port=22/tcp
# firewall-cmd --reload
```

- user 
user needs to use same username and password in each nodes


## Files Structure
```
.
├── inventory               # Target hosts
└── playbook.yml            # The playbook to config servers to ssh without pw

```

## Usage
```
$ ansible-playbook playbook.yml -i inventory -u user1 -k
```
-> input target user's password