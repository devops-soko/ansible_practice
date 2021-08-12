## Title
backup each node's log file in nfs file periodically


## Description
this playbook's functions
- get node's info 
- copy each node's log file to /nfs

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

- nfs setting : please refer to https://github.com/devops-soko/ansible/tree/main/config_common_dir


## Files Structure
- ansible
```
/
└── home
     └── soko0305
            └── ansible
                   └── ansible_practice
                               └──log_backup
                                        ├── inventory               # Target hosts
                                        └── playbook.yml            # The playbook to backup each node's log file in nfs file
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
- one time execution
```
$ ansible-playbook /home/soko0305/ansible/ansible_practice/log_backup/playbook.yml -i /home/soko0305/ansible/ansible_practice/log_backup/inventory -u root

```

- periodical execution (execute ansible at everyday 11:50)
```
$ crontab -e

#add this command
50 11 * * * ansible-playbook /home/soko0305/ansible/ansible_practice/log_backup/playbook.yml -i /home/soko0305/ansible/ansible_practice/log_backup/inventory -u root



#confirm
$ crontab -l
```

