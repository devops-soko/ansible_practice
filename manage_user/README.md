### Title
Manage linux users by ansible

## Description
There are 3 functions in this ansible role
- add user
- remove user
- debug

## Environment

실행환경에 대해 작성하면 된다. OS나 컴파일러 혹은 Hardware와 관련된 환경을 작성하면 된다. Multicore 환경에서 돌아가는 프로그램이라면 CPU나 RAM 같은 것들을 작성해도 좋다.

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

작성한 코드를 어떻게 실행해야 하는지에 대한 가이드라인이다. Usage Example을 함께 작성하면 좋다.

이 외에도 라이센스, contributing 같은 것들도 있지만 처음부터 readme를 복잡하게 작성하기 보단 프로젝트의 규모가 커지면서 디테일하게 추가하며 다듬는 것이 좋다.
