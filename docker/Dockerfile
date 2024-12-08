FROM python:3.13-alpine3.20

RUN set eux; \
  apk add --update --no-cache \
  gcc \
  jq \
  libc-dev \
  libffi-dev \
  bash \
  openssh \
  openssl \
  sshpass \
  git \
  grep \
  curl \
  rsync \
  tree; \
  python3 -m pip install --upgrade pip; \
  python3 -m pip install ansible==10.7.0 ansible-core==2.17.7 ansible-lint; \
  ansible-galaxy collection install ansible.netcommon; \
  ansible-galaxy collection install community.general; \
  addgroup -g 1000 ansible; \
  adduser -u 1000 -G ansible -D ansible -s /bin/bash;

WORKDIR /home/ansible
USER ansible:ansible
ENTRYPOINT []
CMD ["ansible", "--help"]
