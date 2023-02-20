# Scale Service

- Once you have deployed a service to a swarm, and then you want to scale up or scale down the number of containers in the service, you can use the docker service scale command.

- Run the following command to scale the service -

```bash
docker service scale <SERVICE-ID>=<NUMBER-OF-TASKS>
```

**For example :**

Let’s first create 2 services with 2 replicas and 3 replicas respectively -

```bash
vagrant@manager-swarm:/$ docker service create -d --replicas 2 alpine ping 192.168.56.100
j9xr41aqlvxdstcnhp0n1pifp
vagrant@manager-swarm:/$ docker service create -d --replicas 3 alpine ping 192.168.56.100
njfr8n3ou55da1adu5exofrew
vagrant@manager-swarm:/$ docker service ls
ID             NAME             MODE         REPLICAS   IMAGE           PORTS
j9xr41aqlvxd   lucid_taussig    replicated   2/2        alpine:latest
njfr8n3ou55d   magical_merkle   replicated   3/3        alpine:latest
vagrant@manager-swarm:/$
```

To scale the service starting j9, use below command -

```bash
vagrant@manager-swarm:/$ docker service scale j9=7
j9 scaled to 7
overall progress: 7 out of 7 tasks
1/7: running   [==================================================>]
2/7: running   [==================================================>]
3/7: running   [==================================================>]
4/7: running   [==================================================>]
5/7: running   [==================================================>]
6/7: running   [==================================================>]
7/7: running   [==================================================>]
verify: Service converged
vagrant@manager-swarm:/$
```

To scale both the service at a time, use below command -

```bash
vagrant@manager-swarm:/$ docker service scale j9=9 nj=5
j9 scaled to 9
nj scaled to 5
overall progress: 9 out of 9 tasks
1/9: running   [==================================================>]
2/9: running   [==================================================>]
3/9: running   [==================================================>]
4/9: running   [==================================================>]
5/9: running   [==================================================>]
6/9: running   [==================================================>]
7/9: running   [==================================================>]
8/9: running   [==================================================>]
9/9: running   [==================================================>]
verify: Service converged
overall progress: 5 out of 5 tasks
1/5: running   [==================================================>]
2/5: running   [==================================================>]
3/5: running   [==================================================>]
4/5: running   [==================================================>]
5/5: running   [==================================================>]
verify: Service converged
vagrant@manager-swarm:/$
```

Now check the list of services to check the final number of replicas -

```bash
vagrant@manager-swarm:/$ docker service ls
ID             NAME             MODE         REPLICAS   IMAGE           PORTS
j9xr41aqlvxd   lucid_taussig    replicated   9/9        alpine:latest
njfr8n3ou55d   magical_merkle   replicated   5/5        alpine:latest
vagrant@manager-swarm:/$
```
