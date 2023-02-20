# Remove Nodes From Docker Swarm

## Removing a node from the cluster

### Docker swarm leave

```bash
 docker swarm leave
```

output:

```bash
vagrant@node2:~$ docker swarm leave
Node left the swarm.
vagrant@node2:~$
```

- After running this command, worker02 will be detached from the cluster. Now list the node again and see the output:

```bash
vagrant@manager-swarm:~$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
x0ea45l597wbe9up3qcqjj43s     node1           Ready     Active                          23.0.1
smqhgrecy8eyul36ht2utb0bp     node2           Down      Active                          23.0.1
vagrant@manager-swarm:~$
```

Remove a down node from the swarm

```bash
docker node rm node2
```

To forcibly remove an inaccessible node from a swarm, use --force option along with the rm command

```bash
docker node rm --force node1
```
