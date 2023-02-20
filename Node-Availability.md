# Node Availability

To view a list of nodes in the swarm run **docker node** ls from a manager node:

```bash
vagrant@manager-swarm:/$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
x0ea45l597wbe9up3qcqjj43s     node1           Ready     Active                          23.0.1
odcj5iy8975ql03z2kmw00l3o     node2           Ready     Active                          23.0.1
vagrant@manager-swarm:/$
```

The **AVAILABILITY** column shows whether or not the scheduler can assign tasks to the node:

- **Active** means that the scheduler can assign tasks to the node.

- **Pause** means the scheduler doesn’t assign new tasks to the node, but existing tasks remain running.

- **Drain** means the scheduler doesn’t assign new tasks to the node. The scheduler shuts down any existing tasks and schedules them on an available node.

Let’s see an **example** of each of these.

- To check whether the master node is able to assign any task to the paused worker node or not, we will pause one of the nodes-

```bash
vagrant@manager-swarm:/$ docker node update --availability=pause node2
node2
vagrant@manager-swarm:/$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
x0ea45l597wbe9up3qcqjj43s     node1           Ready     Active                          23.0.1
odcj5iy8975ql03z2kmw00l3o     node2           Ready     Pause                           23.0.1
vagrant@manager-swarm:/$
```

Now lets create service with 3 replicas using below command -

```bash
vagrant@manager-swarm:/$ docker service create -d --replicas 3 alpine ping 192.168.56.100
rk92tpgublq7ghz7idp52xk2y
vagrant@manager-swarm:/$ docker service ls
ID             NAME              MODE         REPLICAS   IMAGE           PORTS
rk92tpgublq7   agitated_fermat   replicated   3/3        alpine:latest
vagrant@manager-swarm:/$ docker service ps rk
ID             NAME                IMAGE           NODE            DESIRED STATE   CURRENT STATE            ERROR     PORTS
xc7qzud5srtm   agitated_fermat.1   alpine:latest   node1           Running         Running 19 seconds ago
czc5dp1okhbm   agitated_fermat.2   alpine:latest   node2           Running         Running 19 seconds ago
phfbjh5h7qpf   agitated_fermat.3   alpine:latest   manager-swarm   Running         Running 19 seconds ago
vagrant@manager-swarm:/$
```

- Here you will notice that the manager node did not send any load on the node 2.

- The master node will again start sending the load on node 2 once it is active.

- Note that docker doesn’t do any rebalancing of the load onto the worker nodes.

- Now, let's drain node 2 using below command:

```bash
vagrant@manager-swarm:/$ docker node update --availability=drain node2
node2
vagrant@manager-swarm:/$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
x0ea45l597wbe9up3qcqjj43s     node1           Ready     Active                          23.0.1
odcj5iy8975ql03z2kmw00l3o     node2           Ready     Drain                           23.0.1
vagrant@manager-swarm:/$
```

Once worker 2 is drained, the containers which were on node 2 will be shifted to the other available nodes. In our case, it would be the master node and node 1.
