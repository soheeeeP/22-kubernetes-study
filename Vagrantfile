# -*- mode: ruby -*-
# vi: set ft=ruby :

## configuration variables ##
# 1. VagrantFile에서 사용할 변수 설정
N = 3 
vm_size = {"cpus" => 1, "memory" => 1024}
## /configuration variables ##

# 2. .configure()로 사용할 api 버전을 지정
Vagrant.configure("2") do |config|
    # 3. do |STATEMENT| ~ end 구문으로 작성
    network_prefix = "10.10.10"                                 # ip로 사용할 network의 prefix (3rd byte까지 설정)
    config.vm.box = "bento/ubuntu-20.04"                        # 사용할 가상머신
    config.vm.synced_folder ".", "/vagrant", disabled: true     # host와 vm의 mount 설정

    (1..N).each do |i|
        config.vm.define "Ubuntu-20.04-node#{i}-vm" do |cfg|
            cfg.vm.host_name = "u-node#{i}"                                 # vm의 hostname
            cfg.vm.network "private_network", ip: "#{network_prefix}.#{i}"  # vm의 ip 할당
            cfg.vm.network "forwarded_port", guest: 22, host: "6001#{i}", auto_correct: true, id: "ssh" # host의 port를 vm의 port로 포워딩
            cfg.vm.provider "virtualbox" do |vb|        # --provider [PROVIDER_NAME] option
                vb.name = "Ubuntu-20.04-node#{i}-vb"    # provider name
                vb.gui = false
                vb.cpus = vm_size["cpus"]               # provider cpu 설정 (변수값 이용)
                vb.memory = vm_size["memory"]           # provider memory 설정 (변수값 이용)
                vb.customize ["modifyvm", :id, "--groups", "/i-node(#{N})-group"] # 생성한 VM을 명시된 그룹으로 분류
            end
        end
    end
end
