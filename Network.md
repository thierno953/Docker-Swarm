# Customize the default ingress network

Inspect the ingress network using docker network inspect ingress

```
docker network inspect ingress
```

Remove the existing ingress network:

```bash
docker network rm ingress
```

Create a new overlay network using the --ingress flag, along with the custom options you want to set. This example sets the MTU to 1200, sets the subnet to 192.100.0.0/16, and sets the gateway to 192.100.0.2

```bash
vagrant@manager-swarm:/vagrant$ docker network create \
  --driver overlay \
  --ingress \
  --subnet=192.100.0.0/16 \
  --gateway=192.100.0.2 \
  --opt com.docker.network.driver.mtu=1200 \
  ingress
ykkrpv9xahqkmthvnaxafhxf5
vagrant@manager-swarm:/vagrant$
```

Inspect the ingress network using docker network inspect ingress

```
vagrant@manager-swarm:/vagrant$ docker network inspect ingress
[
    {
        "Name": "ingress",
        "Id": "ykkrpv9xahqkmthvnaxafhxf5",
        "Created": "2023-02-20T17:09:21.001357147Z",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "192.100.0.0/16",
                    "Gateway": "192.100.0.2"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": true,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "ingress-sbox": {
                "Name": "ingress-endpoint",
                "EndpointID": "82da0a9715586aa570880dc86ff1147c285360526b46686c0bb6061b1fb9ad6f",
                "MacAddress": "02:42:c0:64:00:01",
                "IPv4Address": "192.100.0.1/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.driver.mtu": "1200",
            "com.docker.network.driver.overlay.vxlanid_list": "4101"
        },
        "Labels": {},
        "Peers": [
            {
                "Name": "01fb9ef5fab0",
                "IP": "192.168.56.100"
            },
            {
                "Name": "476ed4adad77",
                "IP": "192.168.56.101"
            },
            {
                "Name": "456da188f2f2",
                "IP": "192.168.56.102"
            }
        ]
    }
]
vagrant@manager-swarm:/vagrant$
```
