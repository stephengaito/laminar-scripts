# Namespaces and using docker commands inside a docker container

My Laminar jobs typically run a number of bash scripts *inside* a docker 
container. This allows me to have tighter control over the installed 
environment in which a particular script runs, compiles, and/or packages 
an artifact. 

It would be nice to be able to use the [docker-out-of-docker 
pattern](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/) 
to ensure that docker commands can be called inside a docker container. 

Unfortunately this *will not work* if docker is running with the [Linux 
namespaces 
opton](https://docs-stage.docker.com/engine/security/userns-remap/) on 
(Which is how I run docker on my machines). The point of running docker 
with the namespaces option on, is to ensure that the "docker-root" inside 
a docker container, is just a "normal" "unprivileged" user on the host. 

Unfortunately, this is not privileged enough to run docker commands from 
inside a docker container. The only way to allow the docker-out-of-docker 
pattern, would be to turn the privilege speration provided by namespace 
*off*. 

This means that any script which needs to run a docker command, needs to 
be split into two parts. One part which *can* be run *inside* a docker 
container, and the other part which "finishes" the action by calling the 
docker command from *outside* of any docker container. 
