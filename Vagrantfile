# -*- mode: ruby -*-
# vi: set ft=ruby :

FORWARD_PORTS = true

Vagrant.configure(2) do |config|
  # VM coordinates
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_url = "https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/14.04/providers/virtualbox.box"
  config.ssh.insert_key = false

  servers = {
    :db => ["192.168.50.4", 27017, 27017],
    :api => ["192.168.50.5", 8080, 8080]
  }

  servers.each do |server_name, value|
    config.vm.define server_name do |s|
      s.vm.hostname = "#{server_name}-vm"
      s.vm.network "private_network", ip: value[0]

      # CPU and memory usage
      s.vm.provider "virtualbox" do |v|
        v.memory = 512
        v.cpus = 1
      end

      # Ansible provisioner
      s.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook-#{server_name}.yml"
        ansible.sudo = true
        ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
        ansible.host_key_checking = false
        ansible.verbose = "vv"
        ansible.limit = "all"
      end

      # Forwarded ports
      if FORWARD_PORTS
        puts "'#{server_name}': forwarded guest #{value[1]} port to host #{value[2]} port"
        s.vm.network "forwarded_port", guest: value[1], host: value[2]
      end
    end
  end
end