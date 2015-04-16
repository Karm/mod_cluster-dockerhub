# mod_cluster-dockerhub
mod_cluster toy dev image offering Apache HTTP Server and mod_cluster smart load balancer built from sources.
The Dockerfile comprises a smoke test that verifies whether the compiled server starts without segfaults.

# Example usage

   docker pull karm/mod_cluster-master-dockerhub
   docker run -d -P -i --name mod_cluster karm/mod_cluster-master-dockerhub
   docker ps
   +++ snip +++
   curl 127.0.0.1:49157/mcm
