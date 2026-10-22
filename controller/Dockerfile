# Nodo de control con Ansible (alternativa a WSL)
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get install -y --no-install-recommends ansible sshpass openssh-client \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /ansible
