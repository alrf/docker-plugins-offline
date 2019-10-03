# docker-plugins-offline
**How to install docker plugin offline (without Internet access). It useful for corporate solutions with restricted Internet access.**

Weave directory contains `plugins.tar.gz` for weave plugin.

Make sure you have docker is installed and running on your server w/o Internet access.
Put the `plugins.tar.gz` archive on server.
Run these commands:
```
docker plugin disable weaveworks/net-plugin:latest_release -f
docker plugin rm weaveworks/net-plugin:latest_release -f
tar -zxf plugins.tar.gz -C /var/lib/docker
service docker restart
docker plugin enable weaveworks/net-plugin:latest_release
```
In case you have `"Network 10.32.0.0/12 overlaps with existing route 10.33.181.0/24 on host"` error in the docker logs,
use command:
```
docker plugin set weaveworks/net-plugin:latest_release IPALLOC_RANGE=172.30.0.0/16
```
before
```
service docker restart
docker plugin enable weaveworks/net-plugin:latest_release
```



### For install and prepare your own plugins.tar.gz archive

Use server with Internet access and install neccessary plugin (for example `weave` plugin):

```
docker plugin install weaveworks/net-plugin:latest_release
```

Pack the directory `/var/lib/docker/plugin` into archive:

```
cd /var/lib/docker
tar -czf plugins.tar.gz plugin
```
