# Using Docker containers inside a Laminar job

The [Laminar documentation](https://laminar.ohwg.net/docs.html) contains a 
section on [using docker](https://laminar.ohwg.net/docs.html#Docker-container-jobs)
in a Laminar job. In the use case shown, shell commands are *piped* into 
the docker container. This works very well *if* you are only piping shell 
commands into docker. This can admitedly represent a very large collection 
of uses. If this matches your needs... use it. 

However *if* you are using a docker image which wraps some other program, 
either directly or via a shell script to be run by docker, unless you are 
very lucky, you will find that your Laminar job never completes and you 
have defunct docker processes. This is because there is no 'PID-1' aware 
process which notices that docker has completed and cleans up. 

I have found that *wrapping* the `docker run` command in a 'PID-1' aware 
program, such as [`dumb-init`](https://github.com/Yelp/dumb-init) solves 
the problem. Using `dumb-init -v` provides additional output which 
helps distinguish what is happening. 

[Dumb-init](https://github.com/Yelp/dumb-init) is a single C source file,
and is very easy to build and install. You can get it here: 

```
    https://github.com/Yelp/dumb-init
```

For my use, the standard docker run typically is:

```
dumb-init -v docker run --rm      \
  -v $PWD:/home/dev               \
  -v $PWORKSPACE/$DIST:/workspace \
  -v $ARCHIVE/$RUN:/archive       \
  -u dev:dev                      \
  -w "/home/dev"                  \
  laminar_:$DIST                  \
  /root/dockerScript
```

Where `/root/dockerScript` is the script required to install all 
dependencies and build or run the required job. 

As another example, here is how I *test* the docker version of the 
[pdf2htmlEX]() tool I help develop: 

```
dumb-init -v docker run --rm -i                                   \
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

Note that since the `pdf2htmlEX` application is not 'PID-1' aware, running 
this command with out the `dumb-init` wrapper, results in defunct docker 
processes and Laminar jobs which never complete. 

---

Alternatively:

```
docker run --rm -i               \
  -v $PWD:/home/dev              \
  -v $WORKSPACE/$DIST:/workspace \
  -v $ARCHIVE/$RUN:/archive      \
  -u dev:dev                     \
  -w "/home/dev"                 \
  laminar_:$DIST                 <<DOCKER_SCRIPT


DOCKER_SCRIPT
```

works by piping the commands to the docker image's shell command, as 
suggested by [Laminar's 
documenation](https://laminar.ohwg.net/docs.html#Docker-container-jobs) 
