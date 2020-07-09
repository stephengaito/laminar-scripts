# Tips and Tricks...

This directory contains a number of markdown files which outline some 
problems together with possible solutions that I have found. 

I have recently abandoned the use of docker (and its daemon running as 
root) and instead use the [Podman](https://podman.io/) tool in rootless 
mode as an (almost) direct replacement of docker. Among other things, 
podman can be used inside podman (though I have not yet tested this). 

Since I develop, test and deploy *inside* OCI images, so that I can 
control the installed environment, one of the most important problems I 
have found is managing the use of podman *inside* Laminar jobs. 
