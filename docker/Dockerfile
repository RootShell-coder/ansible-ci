FROM python:3.11-alpine

RUN set eux; \
  apk add --update --no-cache \
  bash \
  openssh \
  openssl \
  sshpass \
  git \
  grep \
  curl \
  tree; \
  python3 -m pip install --upgrade pip; \
  python3 -m pip install ansible ansible-lint; \
  ansible-galaxy collection install ansible.netcommon; \
  addgroup -S ansible; \
  adduser -S ansible -G ansible -h /home/ansible -s /bin/bash;

USER ansible:ansible
ENTRYPOINT []
CMD ["ansible", "--help"]
