# ansible-ci

[![Build and check image](https://github.com/RootShell-coder/ansible-ci/actions/workflows/docker.yml/badge.svg?branch=master)](https://github.com/RootShell-coder/ansible-ci/actions/workflows/docker.yml) [![Build and check image](https://github.com/RootShell-coder/ansible-ci/actions/workflows/docker.yml/badge.svg?branch=master&event=schedule)](https://github.com/RootShell-coder/ansible-ci/actions/workflows/docker.yml)

- [ansible-core==2.17.7](https://docs.ansible.com/ansible/latest/reference_appendices/release_and_maintenance.html#ansible-core-support-matrix)
- [ansible==10.7.0](#fragment)

## .gitlab-ci.yml

```yml
stages:
  - version_list
  - lint
  - play

image: ghcr.io/rootshell-coder/ansible:latest

services:
  - name: docker:dind
    alias: docker

variables:
  DOCKER_HOST: tcp://docker:2376
  DOCKER_DRIVER: overlay2
  DOCKER_TLS_CERTDIR: "/certs"

  ANSIBLE_PYTHON_INTERPRETER: /usr/bin/python3  # To explicitly configure a Python 3 interpreter
  ANSIBLE_ROLES_PATH: roles                     # Colon-separated paths in which Ansible will search for Roles.
  ANSIBLE_INVENTORY: inventories                # Comma-separated list of Ansible inventory sources
  ANSIBLE_LOG_PATH: logs/ansible.log            # File to which Ansible will log on the controller. When empty logging is disabled.
  ANSIBLE_REMOTE_TMP: /tmp                      # Temporary directory to use on targets when executing tasks
  ANSIBLE_HOST_KEY_CHECKING: "true"             # "False" if you want to avoid host key checking by the underlying connection plugin Ansible uses to connect to the host
  ANSIBLE_FORCE_COLOR: "true"                   # This option forces color mode even when running without a TTY or the "nocolor" setting is True.
  ANSIBLE_STARATEGY: free                       # Are a way to control play execution on the number of hosts given in a playbook
  ANSIBLE_PIPELINING: "true"                    # If supported by the connection plugin, reduces the number of network operations required to execute a module on the remote server
  ANSIBLE_BECOME_EXE: "sudo -s"                 # Executable to use for privilege escalation, otherwise Ansible will depend on PATH.
  ANSIBLE_BECOME_USER: ansible                  # The user your login/remote user "becomes" when using privilege escalation, most systems will use "root" when no user is specified
  ANSIBLE_FORCE_HANDLER: "true"                 # This option controls if notified handlers run on a host even if a failure occurs on that host
  ANSIBLE_STDOUT_CALLBACK: yaml                 # Set the main callback used to display Ansible output. You can only have one at a time.
  ANSIBLE_ASK_PASS: "false"                     # This controls whether an Ansible playbook should prompt for a login password
  ANSIBLE_BECOME_ASK_PASS: "flase"              # Toggle to prompt for privilege escalation password.
  ANSIBLE_NOCOWS: "true"                        # If you have cowsay installed but want to avoid the ‘cows’ (why????), use this.
  ANSIBLE_ENABLE_TASK_DEBUGGER: "false"         # Use the debugger keyword for more flexibility.
  ANSIBLE_SSH_RETRIES: 6                        # Ansible retries connections only if it gets an SSH error with a return code of 255.
  # Any other ansible configuration variables

ansible:version_list:
  stage: version_list
  when: manual
  script:
    - ansible --version
    - ansible-lint --version

ansible:lint:
  stage: lint
  script:
    - tree
    - ansible-lint -q --offline

ansible:play:
  stage: play
  script:
    - ansible-playbook playbooks/other_play.yml
```
