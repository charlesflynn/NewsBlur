# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

node_defaults = {
    :domain => 'localdomain',
    :cpus => '1',
    :memory => '768',
    :box => 'precise64',
    :box_url => 'http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box'
}

hostsyaml = './config/hosts.yml'
yaml_nodes = YAML::load_file(hostsyaml)
vagrant_nodes = []
YAML::load_file(hostsyaml).each do |key,value|
    node = node_defaults.merge({ :name => key, :ip => value[0] })
    node[:fqdn] = node[:name] + '.' + node[:domain]
    vagrant_nodes.push(node)
end

Vagrant.configure("2") do |config|

    vagrant_nodes.each do |node|
        config.vm.define node[:name] do |node_config|
            node_config.vm.hostname = node[:name]
            node_config.vm.box = node[:box]
            node_config.vm.box_url = node[:box_url]
            node_config.vm.network :private_network, ip: node[:ip]
            node_config.vm.provision :hosts

            node_config.vm.provider :virtualbox do |vb|
                if  node[:cpus].to_i > 1
                    vb.customize ["modifyvm", :id, "--ioapic", "on"]
                end
                vb.customize ["modifyvm", :id, "--cpus", node[:cpus]]
                vb.customize ["modifyvm", :id, "--memory", node[:memory]]
                vb.customize ["modifyvm", :id, "--name", node[:name]]
            end
        end
    end
end
