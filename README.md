# Ansible


## 1. What is Ansible
### 1) Definition 
- Config Management Tool

### 2) Other Tools 
- chef
- puppet
- salt

### 3) Features
- Python-Friendly : Ansible is a tool implemented by Python -> so Ansible can be used with Python easily
- Low Technology Complexity : simple + easy to use + easy to understand 
                              (ansible's task can be defined by YAML file and YAML file is very simple and easy) 
- usability & scalability : there are a lot of modules and plugin
- Agentless : Ansible needs to be installed on only control node
              It is not nesscary on client node
- Idempotency : producing the same results even though executing multiple times 

### 4) Reference
- Document : https://docs.ansible.com/
- ansible glalaxy : https://galaxy.ansible.com/

### 5) Related Concepts
- inventory : the file to define target host
- playbook : the file that defiens and execute the necessary tasks
- role : Predefined tasks in directory form -> can be called in playbook
- module : a reusable & standalone script that handles tasks

cf. redhat ansible tower & AWS = it is tool to manage ansible servers when user uses a lot of ansible servers to manage 


## 2. How to install Ansible
cf. install ansible on only main control server (it is unnecessary to install ansible on nodes)

### step 1. install epel package
```
# yum  install -y epel-release
```

### step 2. install ansible
```
# yum install ansible
```

### step 3. confirm ansible is installed successfully
```
# ansible --version
```

## 3. the files related to ansible
### 1) /etc/ansible
- /etc/ansible/ansible.cfg : a config file for ansible 
- /etc/ansible/hosts : define target nodes
### 2) /home/user directory/.ansible


## 4. ansible command
### 1) ansbile
- use case: execute module once on target nodes
- frequently used options

| Option | Description | 
|:--------|:--------|
| -i | put inventory |
| -m | select module |
| -a | put moule's args |
| -k | ask for connection password |
| -K | ask for privilege escalation password |
| --list-hosts | outputs a list of matching hosts (does not execute anything else) |

- examples
```
ansible all -m user -a “user=soko password=1234” -k
ansible all -m ping -k
```

### 2) ansible-playbook
- use case: 
- frequently used options

| Option | Description | 
|:--------|:--------|
| -i | put inventory |
| --tags | only run tasks tagged with input values |
| --skip-tags | skip tasks tagged with input values |
| -e | set additional variables |
| -u | remote user |
| -k | ask for connection password |
| --start-at-task | start the playbook at the task matching this name |
| --list-hosts | outputs a list of matching hosts (does not execute anything else) |
| --list-tasks | list all tasks that would be executed (does not execute anything else) |
| --check | try to predict some of the changes that may occur (does not execute anything else) |
| --diff --check | predict the differences in files (does not execute anything else) |

### 3) ansible-galaxy
- use case : use roles in ansible galaxy
- how to use 
```
# ansible-galaxy install [roles name]
```

### 4) ansible-vault
- use case : Encryption
- how to use 
step1. define parameter in playbook or main.yml in tasks directory of roles
```
password:'{{vault_password}}'
```

step2. save parameter's value in file or main.yml in var directory of roles
```
vault_password: test123
```

step3. execute command
```
# ansible-vault encrypt [filename]
```

step4. change file
```
# ansible-vault edit [filename]
```

cf. There are more ansible commands and options. If you want to know more then please refer to https://docs.ansible.com/ansible/latest/user_guide/command_line_tools.html


## 5. inventory
### 1) set node group and name
```
[all] 
web1 ansible_ssh_host=web1.example.co
web2 ansible_ssh_host=web2.example.co
db1 ansible_ssh_host=db1.example.co
db2 ansible_ssh_host=db2.example.co

[southeast]
web1
web2

[northeast]
db1

[southwest]
db2

[east:children]
southeast
northeast
```

### 2) define variable
- make group and define variable at the same time
```
[backup]
db2 backup_file=/tmp/backup_file
```

- define variable in existing group
```
[southeast:vars]
web_file=/tmp/web_file
```

- example
```
---
- hosts: web
  tasks:
  - name: create a web file
    file:
      dest: '{{web_file}}'
      state: '{{file_state}}'

- hosts: backup
  tasks:
  - file:
     dest: '{{backup_file}}'
     state: '{{file_state}}'

- hosts: db
  tasks:
  - file:
     dest: '{{db_file}}'
     state: '{{file_state}}'
    when: db_file is defined
- hosts: all
  tasks:
  - file:
     dest: '{{all_file}}'
     state: '{{file_state}}'
```
-> the values of backup_file and web_file will be defined automatically by inventory file

## 6. yml syntax
### 1) structure
```
---                       # playbook need to start by --- / use space not tab
- name: sample_playbook   
  hosts: all              # target nodes
  become: yes              
  become_user: root     
  tasks:                  # tasks which will be executed by playbook
  - name: ensure apache is at the latest ver  # first task
    yum:                  # first task's module   
     name: httpd   
     state: latest
  - name: ensuer apache is running # second task
    service:          # second task's module
     name: httpd  
     state: started
```
### 2) how to get user input
- define 
```
‘{{var_name}}’
```
- input
```
# ansible-playbook -e var_name=value
```

### 3) tasks external commands
- name : the playbook's name
- hosts : target nodes or group
- become : Escalate Privileges
- become_user : Set user which is going to access the target nodes
- connection : connection type (ssh (defualt) / local)
- tasks : Define the tasks that will be run in that playbook
- handers : Predefined tasks to call in tasks block(like function)
- vars : define variables
- strategy : choose whether to run in order or at random (linear(default) / free)
(refer to https://docs.ansible.com/ansible/latest/user_guide/playbooks_strategies.html)
- forks : Limit number of nodes that can be executed simultaneously (default : 5)
- serial : Number of nodes to run simultaneously 
(refer to https://medium.com/devops-srilanka/difference-between-forks-and-serial-in-ansible-48677ebe3f36)

### 4) tasks internal commands
- name : the task's name
- tags : tag the task 
- register : save the task's output
- when : execute the task when certain conditions are met
- notify : use tasks predefined in handlers
- template : create files by inserting variables into the template (how to make template? please refer to chapter 8)
- loop : execute the task repeatly(refer to https://moonstrike.github.io/ansible/2016/10/17/Ansible-Loops.html)
cf. example
```
---
- name: file management ansible playbook
  hosts: all
  tasks:
  - name : get system var                   #시스템 변수 설정
    ansible.builtin.debug:
      var: ansible_facts
    tags:
    - mkdir
    - mkfile
    - rmfile

  - name: make dir
    file:
      dest: '{{file_path}}'
      state: directory
    tags:
    - mkdir
    register: result

  - name: make file
    file:
      dest: '{{file_path}}'
      state: touch
    tags:
    - mkfile
    register: result


  - name: delete file
    file:
      dest: '{{file_path}}'
      state: absent
    when: ansible_facts['os_family']=="Debian"    #시스템변수 중 특정 변수 비교
    tags:
    - rmfile
    register: result


  - name: debug
    debug: var=result
    tags:
    - mkdir
    - mkfile
    - rmfile
```



### 5)
### 6)
### 7)
### 8)


## 7. roles

## 8. template 
