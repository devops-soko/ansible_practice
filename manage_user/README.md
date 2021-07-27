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
Linux Version : CentOS 8.3.2011
Ansible Version : ansible 2.9.21


## Prerequisite
ssh

작성한 코드를 실행하기 전에 설치해야할 pakage나 의존성이 걸리는 문제들을 설명하면 된다.


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
ansible-playbook ./playbook.yml -i ./inventory -u root -t adduser
```
 
- remove user
```
ansible-playbook ./playbook.yml -i ./inventory -u root -t rmuser
```
