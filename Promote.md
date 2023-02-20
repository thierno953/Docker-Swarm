# Promote A Worker Node To Master

- Promoting a node
  You can promote any worker node to manager status at any time. To promote a node, run the below command on the manager node-

```bash
docker node promote NODE
```

first list the node.

```bash
vagrant@manager-swarm:~$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
x0ea45l597wbe9up3qcqjj43s     node1           Ready     Active                          23.0.1
odcj5iy8975ql03z2kmw00l3o     node2           Ready     Active                          23.0.1
vagrant@manager-swarm:~$
```

- notice the manager status of node1 and node2 its blank in above output. now promote the node1 and node2 using below command.

```bash
vagrant@manager-swarm:~$ docker node promote node1 node2
Node node1 promoted to a manager in the swarm.
Node node2 promoted to a manager in the swarm.
vagrant@manager-swarm:~$
```

- List down the nodes again.

```bash
vagrant@manager-swarm:~$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
x0ea45l597wbe9up3qcqjj43s     node1           Ready     Active         Reachable        23.0.1
odcj5iy8975ql03z2kmw00l3o     node2           Ready     Active         Reachable        23.0.1
vagrant@manager-swarm:~$
```

In the output, you can now see the **MANAGER STATUS** as a new column added. Now, you can run the docker node ls command on newly-promoted 2 nodes as well.

- **Note**: Here we have promoted 2 more nodes to manager status that means now we have a total of three managers. This is because docker recommends an odd number of managers.
