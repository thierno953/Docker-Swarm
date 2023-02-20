# Deploying a Replicated Service

- A replicated service is a Docker Swarm service that has a specified number of replicas running. These replicas consist of multiple instances of the specified Docker container.

- To create our new service, we'll use the docker command along with the service create options. The following command will create a service with 4 replicas -

```bash
vagrant@manager-swarm:/$ docker service create -d --replicas 4 alpine ping 192.168.56.100
hbzenquu2it9m490sozff9v9w
vagrant@manager-swarm:/$
```

- To check all the containers replicas deployed by the service using the below command -

* **docker service ps serviceID**

example:

```bash
vagrant@manager-swarm:/$ docker service ls
ID             NAME                  MODE         REPLICAS   IMAGE           PORTS
hbzenquu2it9   distracted_mccarthy   replicated   4/4        alpine:latest
vagrant@manager-swarm:/$ docker service ps hb
ID             NAME                    IMAGE           NODE            DESIRED STATE   CURRENT STATE            ERROR     PORTS
mhigfarbo1lx   distracted_mccarthy.1   alpine:latest   node1           Running         Running 19 seconds ago
jg2pa7ipqmq1   distracted_mccarthy.2   alpine:latest   node1           Running         Running 19 seconds ago
adaz8n0md4pz   distracted_mccarthy.3   alpine:latest   node2           Running         Running 20 seconds ago
c3obz6owyeqg   distracted_mccarthy.4   alpine:latest   manager-swarm   Running         Running 19 seconds ago
vagrant@manager-swarm:/$
```

- In the output you can see, two containers are running on node1, one on master, and 1 one on node2.

- Now, go to worker01 and remove the underlying containers.

```bash
vagrant@node1:~$ docker container ls
CONTAINER ID   IMAGE           COMMAND                 CREATED         STATUS         PORTS     NAMES
c632cd0406f0   alpine:latest   "ping 192.168.56.100"   2 minutes ago   Up 2 minutes             distracted_mccarthy.2.jg2pa7ipqmq147jrhjcg2ob4t
daac736fc396   alpine:latest   "ping 192.168.56.100"   2 minutes ago   Up 2 minutes             distracted_mccarthy.1.mhigfarbo1lx4rgzeg9pnsk8h
vagrant@node1:~$ docker container rm c6 da -f
c6
da
vagrant@node1:~$
```

And as a result of this, the docker service on the manager node will again start 2 more containers as the replicas are set as 4.

```bash
vagrant@manager-swarm:/$ docker service ps hb
ID             NAME                        IMAGE           NODE            DESIRED STATE   CURRENT STATE                ERROR                         PORTS
36qym7dpme3g   distracted_mccarthy.1       alpine:latest   node1           Running         Running 4 seconds ago
mhigfarbo1lx    \_ distracted_mccarthy.1   alpine:latest   node1           Shutdown        Failed 9 seconds ago         "task: non-zero exit (137)"
skjnlw6q7szv   distracted_mccarthy.2       alpine:latest   manager-swarm   Running         Running 4 seconds ago
jg2pa7ipqmq1    \_ distracted_mccarthy.2   alpine:latest   node1           Shutdown        Failed 9 seconds ago         "task: non-zero exit (137)"
adaz8n0md4pz   distracted_mccarthy.3       alpine:latest   node2           Running         Running 3 minutes ago
fgbi18w49u6u   distracted_mccarthy.4       alpine:latest   manager-swarm   Running         Running about a minute ago
c3obz6owyeqg    \_ distracted_mccarthy.4   alpine:latest   manager-swarm   Shutdown        Failed about a minute ago    "task: non-zero exit (137)"
vagrant@manager-swarm:/$
```

In this way, the docker swarm manager makes sure that the cluster is always running in the desired state.
