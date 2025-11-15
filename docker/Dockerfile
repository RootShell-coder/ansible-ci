ARG PYTHON_VER=3.6.8

FROM python:${PYTHON_VER}-slim

ARG ANSIBLE_VER=2.9.27
ARG COMMUNITY_GENERAL_VER=8.6.11
ARG NETCOMMON_VER=v5.3.0

ENV HOSTNAME=ansible

LABEL org.opencontainers.image.description="Ansible CI container with Python ${PYTHON_VER}, Ansible ${ANSIBLE_VER}, and network automation collections"

COPY .vimrc /root

RUN set -eux; \
  echo "deb [trusted=yes] http://archive.debian.org/debian stretch main" > /etc/apt/sources.list; \
  echo "deb [trusted=yes] http://archive.debian.org/debian-security stretch/updates main" >> /etc/apt/sources.list; \
  echo 'Acquire::Check-Valid-Until "false";' | tee /etc/apt/apt.conf.d/99ignore-release-date; \
  apt-get update; apt-get install --no-install-recommends -y \
    debian-archive-keyring \
    jq \
    bash-completion \
    vim \
    openssh-client \
    openssl \
    ca-certificates \
    sshpass \
    tzdata \
    git \
    grep \
    curl \
    rsync \
    tree \
    nmap \
    unzip \
    sudo \
    dnsutils \
    python3-apt\
    python3-venv; \
  apt-get clean; \
  rm -rf /var/lib/apt/lists/*; \
  rm -f /etc/apt/apt.conf.d/99ignore-release-date;

RUN set -eux; \
  python3 -m pip install --upgrade pip setuptools wheel yq boto boto3 docker kubernetes;  \
  python3 -m pip install --no-cache-dir ansible==${ANSIBLE_VER} ansible-lint cryptography==3.4.8; \
  mkdir -p /tmp/collections; \
  cd /tmp/collections; \
  curl -L https://github.com/ansible-collections/ansible.netcommon/archive/refs/tags/${NETCOMMON_VER}.tar.gz -o netcommon.tar.gz && \
  tar xzf netcommon.tar.gz && \
  cd ansible.netcommon-5.3.0 && \
  ansible-galaxy collection build && \
  ansible-galaxy collection install $(find . -name "*.tar.gz" -type f) || true; \
  cd /tmp/collections; \
  curl -L https://github.com/ansible-collections/community.general/archive/refs/tags/${COMMUNITY_GENERAL_VER}.tar.gz -o community_general.tar.gz && \
  tar xzf community_general.tar.gz && \
  cd community.general-${COMMUNITY_GENERAL_VER} && \
  ansible-galaxy collection build && \
  ansible-galaxy collection install $(find . -name "*.tar.gz" -type f) || true; \
  rm -rf /tmp/collections; \
  mkdir -p /root/.vim/colors; \
  curl -o /root/.vim/colors/monokai.vim https://raw.githubusercontent.com/sickill/vim-monokai/master/colors/monokai.vim

RUN set -eux; \
  addgroup --gid 1000 ansible; \
  adduser --uid 1000 --gid 1000 --disabled-password --shell /bin/bash --gecos "Antique fossil" --home /home/ansible ansible; \
  printf '%s\n' \
    'Defaults:ansible !requiretty' \
    'ansible ALL=(ALL) NOPASSWD:ALL' \
    > /etc/sudoers.d/ansible; \
  chmod 440 /etc/sudoers.d/ansible; \
  cp -r /root/.vim /root/.vimrc /home/ansible/; \
  chown -R ansible:ansible /home/ansible

WORKDIR /home/ansible
USER ansible:ansible

ENTRYPOINT []
CMD ["ansible", "--help"]
