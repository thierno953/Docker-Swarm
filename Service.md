# Docker Swarm Service

## Create a service

- Containers can be deployed to the swarm in much the same way as containers are run on a manager node. A service is created and the image that should be used to deploy the container is specified.

* To create a service first open a terminal and ssh into your manager node and run the following command -

```bash
vagrant@manager-swarm:~$ docker service create -d alpine ping 192.168.56.100
l38sftk2zjv51893fid3qjn4a
vagrant@manager-swarm:~$
```

Here, the docker service create command creates the service, and the arguments alpine ping 192.168.25.10 define the service as an Alpine Linux container that executes the command ping 192.168.56.100

## Inspect a service

When you have deployed a service to your swarm, you can run **docker service inspect** to display the details about a service in an easily readable format.

```bash
vagrant@manager-swarm:~$ docker service ls
ID             NAME               MODE         REPLICAS   IMAGE           PORTS
l38sftk2zjv5   heuristic_carson   replicated   1/1        alpine:latest
vagrant@manager-swarm:~$ docker service inspect l3
[
    {
        "ID": "l38sftk2zjv51893fid3qjn4a",
        "Version": {
            "Index": 54
        },
        "CreatedAt": "2023-02-20T13:54:28.21737465Z",
        "UpdatedAt": "2023-02-20T13:54:28.21737465Z",
        "Spec": {
            "Name": "heuristic_carson",
            "Labels": {},
            "TaskTemplate": {
                "ContainerSpec": {
                    "Image": "alpine:latest@sha256:69665d02cb32192e52e07644d76bc6f25abeb5410edc1c7a81a10ba3f0efb90a",
                    "Args": [
                        "ping",
                        "192.168.56.100"
                    ],
                    "Init": false,
                    "StopGracePeriod": 10000000000,
                    "DNSConfig": {},
                    "Isolation": "default"
                },
                "Resources": {
                    "Limits": {},
                    "Reservations": {}
                },
                "RestartPolicy": {
                    "Condition": "any",
                    "Delay": 5000000000,
                    "MaxAttempts": 0
                },
                "Placement": {
                    "Platforms": [
                        {
                            "Architecture": "amd64",
                            "OS": "linux"
                        },
                        {
                            "OS": "linux"
                        },
                        {
                            "OS": "linux"
                        },
                        {
                            "Architecture": "arm64",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "386",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "ppc64le",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "s390x",
                            "OS": "linux"
                        }
                    ]
                },
                "ForceUpdate": 0,
                "Runtime": "container"
            },
            "Mode": {
                "Replicated": {
                    "Replicas": 1
                }
            },
            "UpdateConfig": {
                "Parallelism": 1,
                "FailureAction": "pause",
                "Monitor": 5000000000,
                "MaxFailureRatio": 0,
                "Order": "stop-first"
            },
            "RollbackConfig": {
                "Parallelism": 1,
                "FailureAction": "pause",
                "Monitor": 5000000000,
                "MaxFailureRatio": 0,
                "Order": "stop-first"
            },
            "EndpointSpec": {
                "Mode": "vip"
            }
        },
        "Endpoint": {
            "Spec": {}
        }
    }
]
vagrant@manager-swarm:~$
```

## Service logs

To check the logs of the container inside the deployed service, use below command -

```bash
vagrant@manager-swarm:~$ docker service logs -f l3
heuristic_carson.1.0dprmzy2kq3k@node1    | PING 192.168.56.100 (192.168.56.100): 56 data bytes
```

where l3 is service ID.

- In some cases, the deployed service might run on multiple containers and can be viewed in the logs as above.
