# Datadog and Docker Swarm

## Pre-work

We are going to create a multi-node environment for our swarm to live in. 

## Vagrant file

I created a Vagrantfile to help provision a group of virtual machines. The code is pretty straight forward but the `TLDR` is: 

1. We use a Vagrant configuration to create 1 Manager node and 3 Worker nodes. 
2. There is a vm provisioner with plugins to help ensure that Docker is fully installed on each node during creation.
3. We assign a private_network with an ip of 192.168.33.x to each node. This will help the Docker swarm manager connect to each worker node and be able to communicate to all services. 

Run `vagrant up` to create the vms. 

