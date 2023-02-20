# What is Docker Swarm?

- A Docker Swarm is a group of either virtual machines or physical that are running the Docker application and that have been configured to join together in a cluster. Once a group of machines has been clustered together, you can still run the Docker commands that you're used to, but they will now be carried out by the machines in your cluster.

- The activities of the cluster are controlled by a swarm manager, and machines that have joined the cluster are referred to as nodes.

## Setup Vagrant and connect to manager-swarm server

```shell
 vagrant up

 vagrant ssh manager-swarm
```

## Copy hosts file on ansible-control

```shell
cp /vagrant/hosts_file /etc/hosts
```

## Create a SSH key and copy to all servers

```shell
ssh-keygen

ssh-copy-id localhost

ssh-copy-id node1 && ssh-copy-id node2
```

# Docker Swarm Installation

In this blog, we will Setup a 3 Node Docker Swarm Cluster

## Prerequisites

3 Fresh Deployed Ubuntu servers with docker installed on them and are in the same network. Set up the hostname of each server as - master, node1, and node2

## DNS Configuration

Update the /etc/hosts file in each machine with the IPs of all the 3 nodes so we can resolve names to IP’s

## Initialize the Swarm

Now we will initialize the swarm on the manager node -

```bash
vagrant@manager-swarm:~$ sudo docker swarm init --advertise-addr 192.168.56.100
Swarm initialized: current node (xxingr7agt8s1zxq2y00oyppc) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-58mya2qhw77yiipihiixpbn2df2mye46zxvcnit9a69ywkwy2l-emnc9q8xxjl27id49catbbvnk 192.168.56.100:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

vagrant@manager-swarm:~$
```

- This command makes the host as the master node and it comes to the cluster mode. Also, in the output it has given us a command with the token to be able to add any worker node to this cluster.

- Right now, only one node is there in the cluster which can be checked using below command-

```bash
docker node ls

vagrant@manager-swarm:~$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
vagrant@manager-swarm:~$
```

- To attach the remaining worker nodes to the master node, run below command on the worker nodes -

On node1 -

```bash
vagrant@node1:~$ docker swarm join --token SWMTKN-1-58mya2qhw77yiipihiixpbn2df2mye46zxvcnit9a69ywkwy2l-emnc9q8xxjl27id49catbbvnk 192.168.56.100:2377
This node joined a swarm as a worker.
vagrant@node1:~$
```

On node2 -

```bash
vagrant@node2:~$ docker swarm join --token SWMTKN-1-58mya2qhw77yiipihiixpbn2df2mye46zxvcnit9a69ywkwy2l-emnc9q8xxjl27id49catbbvnk 192.168.56.100:2377
This node joined a swarm as a worker.
vagrant@node2:~$
```

- Both the nodes has been attached in cluster-

```bash
vagrant@manager-swarm:~$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
x0ea45l597wbe9up3qcqjj43s     node1           Ready     Active                          23.0.1
smqhgrecy8eyul36ht2utb0bp     node2           Ready     Active                          23.0.1
vagrant@manager-swarm:~$
```
