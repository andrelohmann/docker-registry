# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure(2) do |config|

  config.vagrant.plugins = ["vagrant-vbguest", "vagrant-hostmanager"]

  # hostmanager configuration
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.include_offline = true

  config.vm.define "box" do |b|

    b.vm.box = "ubuntu/jammy64" # 22.04

    b.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = "2"
      vb.customize ["modifyvm", :id, "--audio", "none"]
    end

    # vagrant-hostmanager is necessary to update /etc/hosts on hosts and guests
    b.vm.network "private_network", ip: "192.168.56.66"
    b.vm.hostname = "registry.lokal"
    #b.hostmanager.aliases = []

    #b.vm.synced_folder ".", "/vagrant", disabled: true

    # auto update guest additions
    b.vbguest.auto_update = true

    # Install docker and apache2-utils
    b.vm.provision "shell", inline: "apt update && apt install apache2-utils docker.io docker-compose-v2 -yqq && usermod -aG docker vagrant"

    # Run the registry container
    b.vm.provision "shell", inline: "docker compose -f /vagrant/docker-compose.yml up -d"

    # Startup the registry
    #b.vm.provision "shell", inline: ""

  end
end
