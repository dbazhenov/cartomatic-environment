# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

################################################################################

API_VERSION=2

################################################################################

parameters  = YAML.load_file('settings.yml')
nodes       = parameters["nodes"]
settings    = parameters["settings"]

################################################################################

Vagrant.configure(API_VERSION) do |config|
  nodes.each do |node_name, node_data|

    config.vm.define node_name, autostart: false do |conf|

      conf.vm.box = node_data["image"] || "ubuntu/trusty64"
      conf.vm.box_check_update = false
      conf.ssh.forward_agent = true

      conf.vm.hostname = "#{node_name}"
      conf.vm.network "private_network", ip: node_data["ip_addr"]

      conf.vm.provider "virtualbox" do |vbox|
        vbox.memory = node_data["memory"] || 512
        vbox.cpus   = node_data["cpus"] || 1
      end

      conf.vm.provision :ansible do |ansible|
        ansible.limit = node_name
        ansible.playbook = File.join(settings['source_path'], settings['playbook'])
        if !(settings["tags"].nil?)
          ansible.tags = settings["tags"]
        end
        ansible.extra_vars = "@#{File.join(settings['source_path'], settings['config_path'])}"
        ansible.groups = {
          "nginx"        => node_name,
          "php"          => node_name,
          "hhvm"         => node_name,
          "apache"       => node_name,
          "mysql"        => node_name,
          "nosql"        => node_name,
          "app:children" => ["php", "hhvm", "apache"],
          "db:children"  => ["mysql"],
          "fe:children"  => ["nginx"],
        }
      end
    end
  end
end

################################################################################
