# Datadog and Docker Swarm

## Pre-work

We are going to create a multi-node environment for our swarm to live in. 

## Vagrant file

I created a Vagrantfile to help provision a group of virtual machines. The code is pretty straight forward but the `TLDR` is: 

1. We use a Vagrant configuration to create 1 Manager node and 3 Worker nodes. 
2. There is a vm provisioner with plugins to help ensure that Docker is fully installed on each node during creation.
3. We assign a private_network with an ip of 192.168.33.x to each node. This will help the Docker swarm manager connect to each worker node and be able to communicate to all services. 

Run `vagrant up` to create the vms. You may have to install the `vagrant-docker-compose` plugins first.

Verify you have the correct configured node vms up by running `vagrant status` you should see a list of your newly created nodes!

## Why Docker Swarm?

Question: How do we make applications in containerized Docker environments work across multiple nodes?

Docker Swarm is a service that manages Docker containers through a network of multiple nodes. One node is designated as a Manager Node (You can have multiple manager nodes, however one is elected as the leader). The rest are designated as worker nodes.  

## Running your swarm environment

* ssh into your manager node `vagrant ssh dd-swarm-manager-1`
* First we need to find our docker-compose.yaml file thats shared in the vms /vagrant directory. `cd /vagrant`
* Lets initialize our docker swarm. Run `docker swarm init`
    * This may return an error, as we are using vagrant there are several ip address and we need to specify which address the swarm cluster should use. Run `docker swarm init --advertise-addr <node-ip>` <node-ip> should be 192.168.33.11
* This returns a command to use for the other nodes to join the swarm cluster:  `docker swarm join --token <long-token> <node-ip>:2377`
* SSH into your other two worker nodes and use this token to join the swarm. 

Your swarm is now all set! To configure your docker stack run `docker stack deploy --compose-file docker-stack.yaml dd-swarm` 
