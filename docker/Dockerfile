FROM python:alpine

RUN set eux; \
  apk add --update --no-cache \
  gcc \
  libc-dev \
  libffi-dev \
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

WORKDIR /home/ansible
USER ansible:ansible
ENTRYPOINT []
CMD ["ansible", "--help"]
