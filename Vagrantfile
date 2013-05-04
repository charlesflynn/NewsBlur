# -*- mode: ruby -*-
# vi: set ft=ruby :

vagrant_nodes = [
    { :name => 'app',   :ip => '10.100.100.11' },
    { :name => 'db',    :ip => '10.100.100.12' },
    { :name => 'task',  :ip => '10.100.100.13' }
]

node_defaults = {
    :cpus => '1',
    :memory => '768',
    :box => 'precise64',
    :box_url => 'http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box'
}

Vagrant.configure("2") do |config|

    vagrant_nodes.each do |node|
        node_defaults.each do |key, value|
            node[key] = value unless node.has_key?(key)
        end

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
