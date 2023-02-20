# Docker Secrets

- In this blog, we will be learning Docker secrets. Docker secrets offer a secure way to store sensitive information such as usernames, passwords, and even files like self-signed certificates, ssh keys.

Consider the below command -

```bash
#docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=mypassword -d mysql:tag
```

- It's not a secure way to share the password through the command line or through the environment variable.

- Let's dive right in and see how to create secrets.

## docker secret create

Creates a secret using standard input or from a file for the secret content.

**Syntax**

```bash
docker secret create [OPTIONS] SECRET [file|-]
```

**Options**`

```bash
--driver , -d: Secret driver
--label , -l: Secret labels
--template-driver: Template driver
```

Creating a secret-

```bash
docker secret secretName -
```

**Do not miss the ‘-’ in the last**, as it reads the standard input.

The above command will expect an input from you which will be the password. Enter the password and hit ctrl+D to exit -

```bash
vagrant@manager-swarm:/$ printf "my super secret password" | docker secret create dbpass -
s0fd3kagl54plo736h80qpbyx
vagrant@manager-swarm:/$ docker secret ls
ID                          NAME      DRIVER    CREATED         UPDATED
s0fd3kagl54plo736h80qpbyx   dbpass              8 seconds ago   8 seconds ago
vagrant@manager-swarm:/$
```

Let's inspect the above password to check if it is exposing any of the provided secrets -

```bash
vagrant@manager-swarm:/$ docker secret inspect dbpass
[
    {
        "ID": "s0fd3kagl54plo736h80qpbyx",
        "Version": {
            "Index": 235
        },
        "CreatedAt": "2023-02-20T15:20:20.157883912Z",
        "UpdatedAt": "2023-02-20T15:20:20.157883912Z",
        "Spec": {
            "Name": "dbpass",
            "Labels": {}
        }
    }
]
vagrant@manager-swarm:/$
```

The secret we created can only be used by the service it is being assigned to.

```bash
vagrant@manager-swarm:/$ docker service create -d --secret dbpass alpine ping 8.8.8.8
tkp1xz5qavv1hbrjt0wt100u3
vagrant@manager-swarm:/$ docker service ls
ID             NAME              MODE         REPLICAS   IMAGE                      PORTS
rk92tpgublq7   agitated_fermat   replicated   3/3        alpine:latest
tkp1xz5qavv1   relaxed_gates     replicated   1/1        alpine:latest
dglsinmfym5x   thirsty_panini    replicated   2/2        thiernos/node-app:latest
vagrant@manager-swarm:/$ docker service ps tk
ID             NAME              IMAGE           NODE      DESIRED STATE   CURRENT STATE            ERROR     PORTS
ltb7kcml43uz   relaxed_gates.1   alpine:latest   node1     Running         Running 23 seconds ago
vagrant@manager-swarm:/$
```

The above command makes sure that this service container only will have the right to use the dbpass secret.

## Where does the docker save the secrets?

- Docker swarm worker nodes save the secrets at the path /run/secrets/. And this can be verified as follows -

* on node1 node where service's container available

```bash
vagrant@node1:~$ docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED          STATUS          PORTS      NAMES
f060b4524a75   alpine:latest              "ping 8.8.8.8"           48 seconds ago   Up 46 seconds              relaxed_gates.1.ltb7kcml43uzckic25hvy0189
11d173388459   thiernos/node-app:latest   "docker-entrypoint.s…"   6 minutes ago    Up 6 minutes    5000/tcp   thirsty_panini.2.y39n4rkmfn51wtgyfv658sc6d
0228aeca3cf9   alpine:latest              "ping 192.168.56.100"    44 minutes ago   Up 44 minutes              agitated_fermat.1.xc7qzud5srtm6o77dkf3g8kv1
vagrant@node1:~$ docker exec -it f060b4524a75 sh
/ # cd /run/secrets/
/run/secrets # ls
dbpass
/run/secrets # cat dbpass
my super secret password/run/secrets #
/run/secrets #
```
