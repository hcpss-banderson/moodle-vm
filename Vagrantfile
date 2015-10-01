# -*- mode: ruby -*-
# vi: set ft=ruby :

# Get some configuration
require 'yaml'
params = YAML.load_file('parameters.yml')

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.92"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.ssh.forward_agent = true
  config.vm.provision "ansible" do |ansible|
    ansible.extra_vars = params
    ansible.playbook  = "ansible/provision.yml"
  end
end
