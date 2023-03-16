# -*- mode: ruby -*-
# vi: set ft=ruby :

HOSTNAME = "dd-swarm"
MANAGER_NODE_COUNT = 1
WORKER_NODE_COUNT = 2

Vagrant.configure("2") do |config|

    # require plugin https://github.com/leighmcculloch/vagrant-docker-compose
    config.vagrant.plugins = "vagrant-docker-compose"

    # install docker and docker-compose
    config.vm.provision :docker
    config.vm.provision :docker_compose

    config.vm.provider :virtualbox do |v|
      v.linked_clone = true
    end
  
    (1..MANAGER_NODE_COUNT).each do |i|
      config.vm.define ("#{HOSTNAME}-manager-#{i}") do |node|
        node.vm.hostname = "#{HOSTNAME}-manager-#{i}"
        node.vm.box = "ubuntu/focal64"
        node.vm.network "private_network", ip: "192.168.33.#{10+i}"
        # For Nginx
        node.vm.network :forwarded_port, guest: 8080, host:8080 
      end
    end
  
    (1..WORKER_NODE_COUNT).each do |i|
      config.vm.define ("#{HOSTNAME}-worker-#{i}") do |node|
        node.vm.hostname = "#{HOSTNAME}-worker-#{i}"
        node.vm.box = "ubuntu/focal64"
        node.vm.network "private_network", ip: "192.168.33.#{20+i}"
      end
    end
  end