# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  # this same file is used for ansible inventory
  inventory_path = 'inventory/dev' 
  inventory = YAML.load_file(inventory_path)
  ansible_hosts = inventory['all']['hosts']

  ansible_limit = ansible_hosts.map{ | host_name, host | "#{host_name}" }.join(",") || 'all'

  # centos works
  #config.vm.box = "centos/7"
  #config.vm.box_version = "1801.02" # 7.4.1708

  # Alphine Linux requires a plugin to provision 
  #   vagrant plugin install vagrant-alpine
  # and python installed; see shell provision below
  config.vm.box = 'maier/alpine-3.6-x86_64'
  config.vm.box_version = '3.6.2'

  # The offical alpine/alpine does not provision
  # You get the following error: 
  #   Vagrant attempted to execute the capability 'change_host_name
  #config.vm.box = 'alpine/alpine64'
  #config.vm.box_version = '3.6.0'

  ansible_hosts.each_with_index do | (host_name, host_vars), index|
    config.vm.define host_name do |host|
      host.vm.network 'private_network', ip: host_vars["ansible_ip"]
      host.vm.hostname = host_name
      host.hostsupdater.aliases = host_vars["aliases"]
      host.ssh.insert_key = false
      host.vm.synced_folder ".", "/vagrant", disabled: true

      host.vm.provider "virtualbox" do |vb|
        vb.name = host_name
        vb.memory = host_vars["memory"]
        vb.cpus = host_vars[ "cpus" ]
        vb.linked_clone = true
      end

      # alpine only install python for ansible 
      host.vm.provision "shell", inline: "sudo apk --update --no-cache add python"

      # only provision the last host, ansible will manage all hosts
      if index == (ansible_hosts.size-1)
        host.vm.provision "ansible" do |ansible|
          ansible.inventory_path = inventory_path
          ansible.verbose = "v"
          ansible.limit = ansible_limit
          ansible.playbook = "setup.yml"
        end
      end
    end
  end

end
