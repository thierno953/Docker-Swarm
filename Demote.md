# Demoting a node

You can demote any manager node to a worker node at any time. To demote a node, run the below command on the manager node-

```bash
docker node demote NODE
```

**Example**

```bash
vagrant@manager-swarm:~$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
x0ea45l597wbe9up3qcqjj43s     node1           Ready     Active         Reachable        23.0.1
odcj5iy8975ql03z2kmw00l3o     node2           Ready     Active         Reachable        23.0.1
vagrant@manager-swarm:~$
```

here all the node is manager node you can see in manager status.

- You can use node inspect command to check node status whether it's a manager or a worker node -

```bash
vagrant@manager-swarm:~$ docker node demote node1 node2
Manager node1 demoted in the swarm.
Manager node2 demoted in the swarm.
vagrant@manager-swarm:~$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
x0ea45l597wbe9up3qcqjj43s     node1           Ready     Active                          23.0.1
odcj5iy8975ql03z2kmw00l3o     node2           Ready     Active                          23.0.1
vagrant@manager-swarm:~$ 


```
