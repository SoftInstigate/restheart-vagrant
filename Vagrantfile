# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  # VM coordinates
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_url = "https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/14.04/providers/virtualbox.box"
  config.vm.hostname = "restheart"

  # CPU and memory usage
  config.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 2
  end

  # Ansible provisioner
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.verbose = "v"
  end

  # Forwarded ports for MongoDB and HTTP server
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 27017, host: 27017
  config.vm.network "forwarded_port", guest: 28017, host: 28017

end
