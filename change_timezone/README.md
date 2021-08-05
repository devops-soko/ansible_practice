## Title
Change nodes' timezone


## Description
There are 2 functions in this ansible role
- change timezone
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
└── playbook.yml            # The playbook to change timezone
```

## Usage
- check timezone
```
# timedatectl list-timezones

#output example#
# timedatectl list-timezones
Africa/Abidjan
Africa/Accra
Africa/Addis_Ababa
Africa/Algiers
Africa/Asmara
Africa/Bamako
Africa/Bangui
Africa/Banjul
Africa/Bissau
Africa/Blantyre
Africa/Brazzaville

```

- check current timezone 
```
# timedatectl


#output example#
               Local time: Wed 2021-07-28 15:47:12 JST
           Universal time: Wed 2021-07-28 06:47:12 UTC
                 RTC time: Wed 2021-07-28 06:47:12
                Time zone: Asia/Tokyo (JST, +0900)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no


```
 
- change timezone
```
$ ansible-playbook playbook.yml -i inventory  -u root -e timezone_area=Asia/Tokyo
```
