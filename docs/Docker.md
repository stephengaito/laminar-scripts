# Using OCI containers inside a Laminar job

I have recently abandoned the use of docker (and its daemon running as 
root) and instead use the [Podman](https://podman.io/) tool in rootless 
mode as an (almost) direct replacement of docker. Among other things, 
podman can be used inside podman (though I have not yet tested this). 

The [Laminar documentation](https://laminar.ohwg.net/docs.html) contains a 
section on [using 
docker](https://laminar.ohwg.net/docs.html#Docker-container-jobs) in a 
Laminar job. In the use case shown, shell commands are *piped* into the 
OCI container. 

Instead of pipping commands, for my use, the standard podman run typically 
is: 

```
podman run --rm                   \
  -v $PWD:/home/dev               \
  -v $PWORKSPACE/$DIST:/workspace \
  -v $ARCHIVE/$RUN:/archive       \
  -u dev:dev                      \
  -w "/home/dev"                  \
  laminar_:$DIST                  \
  /root/aScript
```

Where `/root/aScript` is the script required to install all 
dependencies and build or run the required job. 

As another example, here is how I *test* the OCI containerized version of 
the [pdf2htmlEX](https://github.com/pdf2htmlEX/pdf2htmlEX) tool I help 
develop: 

```
podman run --rm -i                                                \
  -v $PWD:/home/dev                                               \
  -v $PWORKSPACE/$DIST:/workspace                                 \
  -v $ARCHIVE:/archive                                            \
  -v /home/dev/pdf2htmlexWork/examples:/examples:ro               \
  -v /home/dev/pdf2htmlexWork/pdf2htmlEX/pdf2htmlEX/test:/test:ro \
  pdf2htmlex/pdf2htmlex:latest                                    \
  --zoom 1.3                                                      \
  --embed cfij --bg-format $bgFormat                              \
  --split-pages 1                                                 \
  --dest-dir $PDF_BASE_DIR                                        \
  --page-filename $PDF_NAME-%d.page                               \
  $PDF_PATH.pdf
```

Alternatively:

```
podman run --rm -i               \
  -v $PWD:/home/dev              \
  -v $WORKSPACE/$DIST:/workspace \
  -v $ARCHIVE/$RUN:/archive      \
  -u dev:dev                     \
  -w "/home/dev"                 \
  laminar_:$DIST                 <<PODMAN_SCRIPT


PODMAN_SCRIPT
```

works by piping the commands to the podman image's shell command, as 
suggested by [Laminar's 
documenation](https://laminar.ohwg.net/docs.html#Docker-container-jobs) 
