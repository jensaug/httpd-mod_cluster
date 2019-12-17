## Notes
 
 * 2019-12-17 - Modified to build httpd 2.2.23 and mod_cluster-1.3.10.Final 

## httpd-mod_cluster
httpd-mod_cluster toy dev image offering Apache HTTP Server and mod_cluster smart load balancer built from sources.
The Dockerfile comprises a smoke test that verifies whether the compiled server starts without segfaults.

## Configuration

You can configure the `mod_cluster` parameters through following environment variables provided to `docker run` command:

* `MODCLUSTER_PORT` listening port (default: 6666)
* `MODCLUSTER_ADVERTISE` On/Off flag for `ServerAdvertise` (default: On)
* `MODCLUSTER_ADVERTISE_GROUP` Advertise address and port (default: 224.0.1.105:23364)
* `MODCLUSTER_NET` a [Require ip](http://httpd.apache.org/docs/trunk/mod/mod_authz_host.html) value for the listener (default: 172.)
* `MODCLUSTER_MANAGER_NET` a [Require ip](http://httpd.apache.org/docs/trunk/mod/mod_authz_host.html) value for the mod_cluster manager on path /mcm (default: value of MODCLUSTER_NET variable)

The defaults could work in your Docker environment.

## Example usage

### Build

    docker build -t httpd-mod_cluster .

### Test run

    docker run -it --name my-httpd-mod_cluster httpd-mod_cluster
    docker ps
    +++ snip +++
    curl 127.0.0.1:49157/mcm
 
To debug/inspect, one may use: 

    docker exec -i -t my-httpd-mod_cluster bash

### Control configuration using environment variables
This example show how to run a container with host's network stack, disabled advertisement and moc_cluster-manager available only from localhost.

    docker run -it --rm --net host --name my-httpd-mod_cluster \
        -e MODCLUSTER_ADVERTISE=Off \
        -e MODCLUSTER_NET=192.168. \
        -e MODCLUSTER_PORT=7777 \
        -e MODCLUSTER_MANAGER_NET=127.0.0.1 \
        httpd-mod_cluster
    curl 127.0.0.1:6666/mcm
    
   
