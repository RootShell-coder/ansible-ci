FROM python:3-alpine

WORKDIR /srv/ansible

RUN set eux; \
    apk add --update --no-cache \
    ansible \
    ansible-lint \
    bash \
    openssh \
    openssl \
    sshpass \
    git \
    grep \
    curl \
    tree; \
    addgroup -S ansible; \
    adduser -S ansible -G ansible -h /srv/ansible -s /bin/bash;

USER ansible:ansible
ENTRYPOINT []
CMD ["ansible", "--help"]
