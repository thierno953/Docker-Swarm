# Add Nodes In Docker Swarm

Previously we had seen that to join a node to the cluster we need a command with the token provided by the master node. Suppose we lose that token and we want to add a new node into the cluster.

## Adding a new worker node

To add a worker node to the cluster, we will use the below command on the master node -

```bash
 docker swarm join-token worker
```

output:

```bash
vagrant@manager-swarm:~$ docker swarm join-token worker
To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-58mya2qhw77yiipihiixpbn2df2mye46zxvcnit9a69ywkwy2l-emnc9q8xxjl27id49catbbvnk 192.168.56.100:2377

vagrant@manager-swarm:~$
```

To join the worker to the cluster, run the command that we get from the output.

## Adding a new master node

To add a master node to the cluster, we will use the below command on the master node -

```bash
docker swarm join-token manager
```

```bash
vagrant@manager-swarm:~$ docker swarm join-token manager
To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-58mya2qhw77yiipihiixpbn2df2mye46zxvcnit9a69ywkwy2l-eod33nbab1kjjg257f4e8ol53 192.168.56.100:2377

vagrant@manager-swarm:~$
```

Run the above command on any node to add it as a master in the cluster.
