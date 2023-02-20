# Service Create Options

As we have seen, the docker service create command creates a new service. It also provides many more options which can fulfill your various requirements as described by the specified parameters.

**Syntax**

```bash
$ docker service create [OPTIONS] IMAGE [COMMAND] [ARG...]
```


**Options**

* --reserve-cpu: Reserves the CPUs

This parameter can be used to reserve a specific amount of CPU memory for the container.



* --limit-cpu: Limit CPUs

* Similarly, we can also limit our container to use a defined amount of memory and not above that.

* --update-delay: Delay between updates

We can compel the containers to wait for a certain amount of time to wait before updating.

* --update-failure-action : Action on update failure

* --update-max-failure-ratio: Failure rate to tolerate during an update (default 0)

* --update-order: Update order

* --update-parallelism: Maximum number of tasks updated simultaneously (0 to update all at once)


```bash 
docker service create --help | grep resou

docker service create --help | grep reser

docker service create --help | grep limit

docker service create --help | grep update
```