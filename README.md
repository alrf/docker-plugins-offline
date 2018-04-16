# docker-plugins-offline
**How to install docker plugin offline (without Internet access). It useful for corporate solutions with restricted Internet access.**

Weave directory contains `plugin.tar.gz` for weave plugin. 

Make sure you have docker is installed and running on your server w/o Internet access.
Put the `plugin.tar.gz` archive on server.
Run these commands:
```
docker plugin disable weaveworks/net-plugin:latest_release -f
docker plugin rm weaveworks/net-plugin:latest_release -f
tar -zxf plugins.tar.gz -C /var/lib/docker
rm -f /var/lib/docker/plugins/475fbe3a6e2eb5ab997493aea88fb9bd5f981ba2f9c8b403b430d17c7adc19c6/rootfs/restart.sentinel
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



### For install and prepare your own plugin.tar.gz archive

Use server with Internet access and install neccessary plugin (for example `weave` plugin):

```
docker plugin install weaveworks/net-plugin:latest_release
```

Pack the directory `/var/lib/docker/plugin` into archive:

```
cd /var/lib/docker
tar -czf plugin.tar.gz plugin
```
