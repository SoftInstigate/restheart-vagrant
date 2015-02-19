# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # VM coordinates
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_url = "https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/14.04/providers/virtualbox.box"

  config.vm.define "db" do |db|
    db.vm.hostname = "db-vm"
    db.vm.network "private_network", ip: "192.168.50.4"

    # CPU and memory usage
    db.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
    end

    # Ansible provisioner
    db.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook-db.yml"
      ansible.verbose = "vv"
    end

    # Forwarded ports for MongoDB
    db.vm.network "forwarded_port", guest: 27017, host: 37017
    db.vm.network "forwarded_port", guest: 28017, host: 38017
  end

  config.vm.define "api" do |api|
    api.vm.hostname = "api-vm"
    api.vm.network "private_network", ip: "192.168.50.5"

    # CPU and memory usage
    api.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
    end

    # Ansible provisioner
    api.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook-api.yml"
      ansible.verbose = "vv"
    end

    # Forwarded ports for MongoDB and HTTP server
    api.vm.network "forwarded_port", guest: 4443, host: 4443
    api.vm.network "forwarded_port", guest: 8080, host: 8080

    api.vm.synced_folder "tmp", "/tmp"
  end

end