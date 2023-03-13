# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|


    # require plugin https://github.com/leighmcculloch/vagrant-docker-compose
    config.vagrant.plugins = "vagrant-docker-compose"

    # install docker and docker-compose
    config.vm.provision :docker
    config.vm.provision :docker_compose

    config.vm.provider :virtualbox do |v|
      v.linked_clone = true
    end
  
    (1..1).each do |i|
      config.vm.define ("m%02d" % i) do |node|
        node.vm.hostname = "m%02d" % i
        node.vm.box = "ubuntu/focal64"
        node.vm.network "private_network", ip: "192.168.33.#{10+i}"
        node.vm.network :forwarded_port, guest: 8080, host: 8080
        node.vm.network :forwarded_port, guest: 5000, host: 5050
        node.vm.network :forwarded_port, guest: 9000, host: 9000
      end
    end
  
    (1..3).each do |i|
      config.vm.define ("w%02d" % i) do |node|
        node.vm.hostname = "w%02d" % i
        node.vm.box = "ubuntu/focal64"
        node.vm.network "private_network", ip: "192.168.33.#{20+i}"
      end
    end
  
  end