ARG PYTHON_VER=3.6.8

FROM python:${PYTHON_VER}-slim

ARG ANSIBLE_VER=2.9.27
ARG COMMUNITY_GENERAL_VER=8.6.1
ARG NETCOMMON_VER=5.3.0

LABEL org.opencontainers.image.description="Ansible CI container with Python ${PYTHON_VER}, Ansible ${ANSIBLE_VER}, and network automation collections"

RUN set -eux; \
  echo "deb [trusted=yes] http://archive.debian.org/debian stretch main" > /etc/apt/sources.list; \
  echo "deb [trusted=yes] http://archive.debian.org/debian-security stretch/updates main" >> /etc/apt/sources.list; \
  apt-get \
    -o Acquire::AllowDowngradeToInsecureRepositories=true \
    -o Acquire::Check-Valid-Until=false \
    -o Acquire::AllowUnsignedPackages=true \
    -o APT::Get::AllowUnauthenticated=true \
     update; \
  apt-get install --no-install-recommends -y \
    -o Acquire::AllowDowngradeToInsecureRepositories=true \
    -o Acquire::Check-Valid-Until=false \
    -o Acquire::AllowUnsignedPackages=true \
    -o APT::Get::AllowUnauthenticated=true \
  jq \
  bash \
  openssh-client \
  openssl \
  ca-certificates \
  sshpass \
  git \
  grep \
  curl \
  rsync \
  tree \
  nmap \
  dnsutils; \
  apt-get clean; \
  rm -rf /var/lib/apt/lists/*;

RUN set -eux; \
  python3 -m pip install --upgrade pip setuptools wheel; \
  python3 -m pip install --no-cache-dir ansible==${ANSIBLE_VER} ansible-lint cryptography==3.4.8; \
  mkdir -p /tmp/collections; \
  cd /tmp/collections; \
  curl -L https://github.com/ansible-collections/ansible.netcommon/archive/refs/tags/${NETCOMMON_VER}.tar.gz -o netcommon.tar.gz && \
  tar xzf netcommon.tar.gz && \
  cd ansible.netcommon-${NETCOMMON_VER} && \
  ansible-galaxy collection build && \
  ansible-galaxy collection install $(find . -name "*.tar.gz" -type f) || true; \
  cd /tmp/collections; \
  curl -L https://github.com/ansible-collections/community.general/archive/refs/tags/${COMMUNITY_GENERAL_VER}.tar.gz -o community_general.tar.gz && \
  tar xzf community_general.tar.gz && \
  cd community.general-${COMMUNITY_GENERAL_VER} && \
  ansible-galaxy collection build && \
  ansible-galaxy collection install $(find . -name "*.tar.gz" -type f) || true; \
  rm -rf /tmp/collections;

RUN set -eux; \
  addgroup --gid 1000 ansible; \
  adduser --uid 1000 --gid 1000 --disabled-password --shell /bin/bash --gecos "Antique fossil" ansible;

WORKDIR /home/ansible
USER ansible:ansible

ENTRYPOINT []
CMD ["ansible", "--help"]
