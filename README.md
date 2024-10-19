# ansible-ci

[![Ansible-CI](https://github.com/RootShell-coder/ansible-ci/actions/workflows/docker-image.yml/badge.svg?branch=master)](https://github.com/RootShell-coder/ansible-ci/actions/workflows/docker-image.yml) [![Ansible-CI](https://github.com/RootShell-coder/ansible-ci/actions/workflows/docker-image.yml/badge.svg?branch=master&event=schedule)](https://github.com/RootShell-coder/ansible-ci/actions/workflows/docker-image.yml)

## .gitlab-ci.yml

```yml
stages:
  - ver
  - lint
  - play

variables:
  ANSIBLE_PYTHON_INTERPRETER: /usr/bin/python3
  ANSIBLE_ROLES_PATH: roles
  ANSIBLE_INVENTORY: inventories
  ANSIBLE_REMOTE_TMP: /tmp
  ANSIBLE_FORCE_COLOR: "true"
  ANSIBLE_STARATEGY: free
  ANSIBLE_PIPELINING: 1
  ANSIBLE_BECOME_EXE: su
  ANSIBLE_BECOME_METHOD: su
  ANSIBLE_BECOME_USER: root
  ANSIBLE_BECOME_ASKPASS: "False"
  ANSIBLE_FORCE_HANDLER: "True"
  ANSIBLE_STDOUT_CALLBACK: yaml
  ANSIBLE_ASK_PASS: "False"
  ANSIBLE_NOCOWS: 1

ansible_ver:
  stage: ver
  image: ghcr.io/rootshell-coder/ansible:latest
  when: manual
  script:
    - ansible --version
    - ansible-lint --version

ansible_lint:
  stage: lint
  image: ghcr.io/rootshell-coder/ansible:latest
  script:
    - tree
    - ansible-lint -q --offline

ansible_play:
  stage: play
  image: ghcr.io/rootshell-coder/ansible:latest
  script:
    - ansible-playbook playbooks/other_play.yml
```
