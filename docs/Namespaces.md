# Namespaces and using podman commands inside an OCI container

I have recently abandoned the use of docker (and its daemon running as 
root) and instead use the [Podman](https://podman.io/) tool in rootless 
mode as an (almost) direct replacement of docker. Among other things, 
podman can be used inside podman (though I have not yet tested this). 

My Laminar jobs typically run a number of bash scripts *inside* an OCI 
container. This allows me to have tighter control over the installed 
environment in which a particular script runs, compiles, and/or packages 
an artifact. 

At the moment, any script which needs to run a podman command, is split 
into two parts. One part which *is* run *inside* an OCI container, and the 
other part which "finishes" the action by calling the podman command from 
*outside* of any OCI container. 
