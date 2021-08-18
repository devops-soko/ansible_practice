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
cf. install ansible on only main control server (it is unnecessary to install ansible on agents)

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
- use case: execute module once on target agents
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

## 6. yml syntax

## 7. roles

## 8. template 
