# Docker Stack

- Till now we had deployed services in a docker using the docker-compose file. But this file only creates the resources on the single node docker host.

- Now suppose there are multiple services to be deployed on multiple nodes.

```bash
vagrant@manager-swarm:/vagrant$ docker node ls
ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xxingr7agt8s1zxq2y00oyppc *   manager-swarm   Ready     Active         Leader           23.0.1
x0ea45l597wbe9up3qcqjj43s     node1           Ready     Active                          23.0.1
odcj5iy8975ql03z2kmw00l3o     node2           Ready     Active                          23.0.1
vagrant@manager-swarm:/vagrant$
```

In such a case manually starting services is not feasible. If we run the below docker-compose file, it will create the containers only on the docker host along with a warning saying ‘The Docker engine you are using is running in swarm mode’

```bash
vagrant@manager-swarm:/vagrant$ cat docker-compose.yml
version: "3.9"
services:
  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
vagrant@manager-swarm:/vagrant$
```

```bash
vagrant@manager-swarm:/vagrant$ docker-compose up -d
WARNING: The Docker Engine you're using is running in swarm mode.

Compose does not use swarm mode to deploy services to multiple nodes in a swarm. All containers will be scheduled on the current node.

To deploy your application across the swarm, use `docker stack deploy`.

Creating network "vagrant_default" with the default driver
Pulling db (mysql:5.7)...
5.7: Pulling from library/mysql
e048d0a38742: Pull complete
c7847c8a41cb: Pull complete
351a550f260d: Pull complete
8ce196d9d34f: Pull complete
17febb6f2030: Pull complete
d4e426841fb4: Pull complete
fda41038b9f8: Pull complete
f47aac56b41b: Pull complete
a4a90c369737: Pull complete
97091252395b: Pull complete
84fac29d61e9: Pull complete
Digest: sha256:8cf035b14977b26f4a47d98e85949a7dd35e641f88fc24aa4b466b36beecf9d6
Status: Downloaded newer image for mysql:5.7
Pulling wordpress (wordpress:latest)...
latest: Pulling from library/wordpress
bb263680fed1: Already exists
0825793cba86: Pull complete
de3c011d207b: Pull complete
7e3c5bd9650e: Pull complete
40c3827232f7: Pull complete
1fdaec518652: Pull complete
5bfea1d79d41: Pull complete
4caaa564818e: Pull complete
62b240c2d643: Pull complete
777fcd639ab6: Pull complete
d5443b496068: Pull complete
9eeb44b3cac4: Pull complete
7138da260462: Pull complete
96dbeb380e49: Pull complete
c3d3dbe884dc: Pull complete
ee27e3611b15: Pull complete
c8157ccf644d: Pull complete
8447bcc57bc9: Pull complete
645182ee3d9e: Pull complete
2035a210545d: Pull complete
97c6208e3281: Pull complete
Digest: sha256:33a5bd6a50f6ff2d9e72e82fb70a274c056f6cd7535af93f1999a5746cef2fce
Status: Downloaded newer image for wordpress:latest
Creating vagrant_db_1 ... done
Creating vagrant_wordpress_1 ... done
vagrant@manager-swarm:/vagrant$
```

- The task is to create services on all the nodes rather than on a single node. For this purpose, the docker stack comes in handy. Docker stack is used to perform actions on the entire cluster.

- The YAML file for both docker-compose and docker stack is similar, just that there are few parameters that are supported in compose file and not in stack file and vice versa.

- Consider the below docker-compose.yml file-

```bash
vagrant@manager-swarm:/vagrant$ cat docker-compose.yml
version: "3.9"
services:
  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
vagrant@manager-swarm:/vagrant$
```

To deploy this file, use below command -

```bash
docker stack deploy - c [filename] [stackname]
```

```bash
vagrant@manager-swarm:/vagrant$ docker stack deploy -c docker-compose.yml wordpress
Ignoring unsupported options: restart

Creating network wordpress_default
Creating service wordpress_db
Creating service wordpress_wordpress
vagrant@manager-swarm:/vagrant$
```

To list all the services, use the below command-

```bash
vagrant@manager-swarm:/vagrant$ docker stack ls
NAME        SERVICES
wordpress   2
vagrant@manager-swarm:/vagrant$
```

To list all the processes in the stack -

```bash
vagrant@manager-swarm:/vagrant$ docker stack ps wordpress
ID             NAME                    IMAGE              NODE      DESIRED STATE   CURRENT STATE                  ERROR     PORTS
nxg000tvhkd1   wordpress_db.1          mysql:5.7          node2     Running         Preparing about a minute ago
iqrjyxai0ke6   wordpress_wordpress.1   wordpress:latest   node1     Running         Preparing about a minute ago
vagrant@manager-swarm:/vagrant$
```

To list the services in the stack -

```bash
vagrant@manager-swarm:/vagrant$ docker stack services wordpress
ID             NAME                  MODE         REPLICAS   IMAGE              PORTS
peugtwr7bgij   wordpress_db          replicated   1/1        mysql:5.7
iclb4jvnpkah   wordpress_wordpress   replicated   1/1        wordpress:latest   *:8000->80/tcp
vagrant@manager-swarm:/vagrant$
```

And to remove the services, use the below command -

```bash
vagrant@manager-swarm:/vagrant$ docker stack rm wordpress
Removing service wordpress_db
Removing service wordpress_wordpress
Removing network wordpress_default
vagrant@manager-swarm:/vagrant$
```
