# """
# Code written by Julien (Yendoupabe) KOLANI
# System and Network Administrator | Maker | Linux Enthusiast
# Passionate about IoT, Electronics, AI, and Programming
# Python & DevOps Enthusiast

# GitHub: @julienkolani
# LinkedIn: in/julien-kolani
# Portfolio: https://julienkolanicv.netlify.app/
# Twitter: @julien_kolani
# Instagram: julien_kolani

# Location: Lomé, Togo

# Created: Thursday, June 27, 2024.
# Last modified: Thursday, June 27, 2024.

# Description:


# "Hello! I really enjoy Linux systems, programming, and data science. 
# In fact, it's IoT, electronics, AI, and programming that interest me. 
# Here, I share my projects, my passion, and my daily life."
# """

# -*- mode: ruby -*-
# vi: set ft=ruby :

# Configuration variables
VAGRANT_BOX = "ubuntu/jammy64"
VM_HOSTNAME = "gitlab"
PRIVATE_IP = "192.168.56.18"
PUBLIC_NETWORK_BRIDGE = "wlp0s20f3"
VM_MEMORY = 4096
GITLAB_VERSION = "latest"
GITLAB_RUNNER_VERSION = "ubuntu"
GITLAB_DOMAIN = "gitlab-ce.com"
GITLAB_NETWORK_SUBNET = "172.20.1.0/24"
GITLAB_NETWORK_GATEWAY = "172.20.1.254"
GITLAB_IP = "172.20.1.33"

Vagrant.configure("2") do |config|
  config.vm.box = VAGRANT_BOX
  config.vm.hostname = VM_HOSTNAME
  config.vm.provider "virtualbox"

  config.vm.network "private_network", ip: PRIVATE_IP
  config.vm.network "public_network", bridge: PUBLIC_NETWORK_BRIDGE

  # Shared folder configuration for persistent data
  config.vm.synced_folder "./gitlab_data", "/vagrant_data", create: true

  config.vm.provision "shell", inline: <<-SHELL
    set -e  # Stop script execution on error


    # Mise à jour du système et installation des prérequis
    sudo apt-get update
    sudo apt-get install -y ca-certificates curl

    # Ajout de la clé GPG officielle de Docker
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc

    # Ajout du dépôt Docker aux sources APT
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    # Mise à jour de la liste des paquets
    sudo apt-get update

    # Installation de Docker
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin


    # Create directories for persistent data
    sudo mkdir -p /srv/gitlab/config /srv/gitlab/logs /srv/gitlab/data
    sudo mkdir -p /srv/gitlab-runner

    # Set correct permissions
    sudo chmod -R 777 /srv/gitlab /srv/gitlab-runner

    # Generate GitLab configuration file
    cat <<EOF > /srv/gitlab/config/gitlab.rb
external_url 'http://#{GITLAB_DOMAIN}'
gitlab_rails['gitlab_shell_ssh_port'] = 2222
registry_external_url 'http://#{GITLAB_DOMAIN}:5050'
gitlab_rails['registry_enabled'] = true
EOF

    # Create Docker Compose file
    cat <<EOF > /vagrant_data/docker-compose.yml
version: '3.7'
services:
  gitlab:
    image: gitlab/gitlab-ce:#{GITLAB_VERSION}
    container_name: gitlab
    restart: always
    hostname: '#{GITLAB_DOMAIN}'
    ports:
      - '80:80'
      - '443:443'
      - '2222:22'
      - '5050:5050'
    volumes:
      - '/srv/gitlab/config:/etc/gitlab'
      - '/srv/gitlab/logs:/var/log/gitlab'
      - '/srv/gitlab/data:/var/opt/gitlab'
    shm_size: '256m'
    networks:
      gitlab-network:
        ipv4_address: #{GITLAB_IP}

  gitlab-runner:
    image: gitlab/gitlab-runner:#{GITLAB_RUNNER_VERSION}
    container_name: gitlab-runner
    restart: always
    volumes:
      - '/srv/gitlab-runner:/etc/gitlab-runner'
      - '/var/run/docker.sock:/var/run/docker.sock'
    networks:
      - gitlab-network

networks:
  gitlab-network:
    driver: bridge
    ipam:
      config:
        - subnet: #{GITLAB_NETWORK_SUBNET}
          gateway: #{GITLAB_NETWORK_GATEWAY}
EOF

    # Start Docker containers
    cd /vagrant_data
    docker compose up -d

  SHELL

  config.vm.network "forwarded_port", guest: 22, host: 2230, id: "ssh"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = VM_MEMORY.to_s
  end
end
