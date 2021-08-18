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
