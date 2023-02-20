# Docker swarm port mapping

To map a port through a service, run the below command -

```bash
vagrant@manager-swarm:/$ docker service create -d -p 8080:80 nginx
8l9mulol1ysxmuf5n634j50sb
vagrant@manager-swarm:/$ docker service ls
ID             NAME             MODE         REPLICAS   IMAGE          PORTS
8l9mulol1ysx   friendly_black   replicated   0/1        nginx:latest   *:8080->80/tcp
vagrant@manager-swarm:/$
```

This means that the Nginx service will be available through each of the IP addresses of the container and the port 8080 in the cluster. lets access it using curl command

```bash
vagrant@manager-swarm:/$ curl node1:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

==============================================

vagrant@manager-swarm:/$ curl node2:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
vagrant@manager-swarm:/$
```

Here the 192.168.56.101 is the ip of the container on node1. And the same service can also be accessed on the master node and the worker node node1

```bash
http://192.168.56.101:8080/

http://192.168.56.102:8080/
```
