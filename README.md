# mod_cluster-dockerhub
mod_cluster toy dev image offering Apache HTTP Server and mod_cluster smart load balancer built from sources.
The Dockerfile comprises a smoke test that verifies whether the compiled server starts without segfaults.

# Example usage -- test run

    docker pull karm/mod_cluster-master-dockerhub
    docker run -d -P -i --name mod_cluster karm/mod_cluster-master-dockerhub
    docker ps
    +++ snip +++
    curl 127.0.0.1:49157/mcm
 
To debug/inspect, one may use: 

    docker exec -i -t mod_cluster bash

# Example usage -- as a base image
One may base one's own images on this ```karm/mod_cluster-master-dockerhub``` as simply as with creating a ```Dockerfile```:

    FROM karm/mod_cluster-master-dockerhub:latest
    MAINTAINER Your identity of choice <your email>
    
    # Your own configuration located in the same directory as your Dockerfile
    ADD mod_cluster.conf ${HTTPD_MC_BUILD_DIR}/conf/extra/mod_cluster.conf
    
Note that ```HTTPD_MC_BUILD_DIR``` as well as other ```ENV ``` constants are propagated to your Dockerfile. 

#Notes
 
 * 2015-06-08 - master branch Docker image updated from Fedora 20 to Fedora 22. Enjoy! 
 * 2015-05-07 - [mod_cluster 1.3.1.Final released](https://developer.jboss.org/wiki/ModclusterVersion131FinalReleased), Includes a fix for [CVE-2015-0298](https://access.redhat.com/security/cve/CVE-2015-0298) and more!
