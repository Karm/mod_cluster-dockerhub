# mod_cluster-dockerhub
mod_cluster toy dev image offering Apache HTTP Server and mod_cluster smart load balancer built from sources.
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

### Test run

    docker pull karm/mod_cluster-master-dockerhub
    docker run -d -P -i --name mod_cluster karm/mod_cluster-master-dockerhub
    docker ps
    +++ snip +++
    curl 127.0.0.1:49157/mcm
 
To debug/inspect, one may use: 

    docker exec -i -t mod_cluster bash

### Control configuration using environment variables
This example show how to run a container with host's network stack, disabled advertisement and moc_cluster-manager available only from localhost.

    docker run -it --rm --net host --name mod_cluster \
        -e MODCLUSTER_ADVERTISE=Off \
        -e MODCLUSTER_NET=192.168. \
        -e MODCLUSTER_MANAGER_NET=127.0.0.1 \
        karm/mod_cluster-master-dockerhub
    curl 127.0.0.1:6666/mcm

### Use as a base image
One may base one's own images on this ```karm/mod_cluster-master-dockerhub``` as simply as with creating a ```Dockerfile```:

    FROM karm/mod_cluster-master-dockerhub:latest
    MAINTAINER Your identity of choice <your email>
    
    # Your own configuration located in the same directory as your Dockerfile
    COPY mod_cluster.conf ${HTTPD_MC_BUILD_DIR}/conf/extra/mod_cluster.conf

    # Your own entry point script located in the same directory as your Dockerfile
    COPY docker-entrypoint.sh /

    
Note that ```HTTPD_MC_BUILD_DIR``` as well as other ```ENV ``` constants are propagated to your Dockerfile. 

## Notes
 
 * 2015-08-31 - control run through environment variables 
 * 2015-06-08 - master branch Docker image updated from Fedora 20 to Fedora 22. Enjoy! 
 * 2015-05-07 - [mod_cluster 1.3.1.Final released](https://developer.jboss.org/wiki/ModclusterVersion131FinalReleased), Includes a fix for [CVE-2015-0298](https://access.redhat.com/security/cve/CVE-2015-0298) and more!
