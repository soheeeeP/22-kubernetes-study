# -*- mode: ruby -*-
# vi: set ft=ruby :

## configuration variables ##
# max number of worker nodes
N = 3 
vm_size = {"cpus" => 1, "memory" => 1024}
## /configuration variables ##

Vagrant.configure("2") do |config|
    network_prefix = "10.10.10"
    config.vm.box = "bento/ubuntu-20.04"
    config.vm.synced_folder ".", "/vagrant", disabled: true

    (1..N).each do |i|
        config.vm.define "Ubuntu-20.04-node#{i}-vm" do |cfg|
            cfg.vm.host_name = "u-node#{i}"
            cfg.vm.network "private_network", ip: "#{network_prefix}.#{i}"
            cfg.vm.network "forwarded_port", guest: 22, host: "6001#{i}", auto_correct: true, id: "ssh"
            cfg.vm.provider "virtualbox" do |vb|
                vb.name = "Ubuntu-20.04-node#{i}-vb"
                vb.gui = false
                vb.cpus = vm_size["cpus"]
                vb.memory = vm_size["memory"]
                vb.customize ["modifyvm", :id, "--groups", "/i-node(#{N})-group"]       
            end
        end
    end
end
