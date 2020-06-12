# General Laminar scripts

This directory provides a collection of general [Laminar (shell) 
scripts](https://laminar.ohwg.net/docs.html#Helper-scripts) which are 
usable in any job. 

- `devDocker` runs a `docker run` command with an internal `dev:dev` 
  user/group in the `/home/dev` working directory. Any command arguments 
  are appended to the `docker run` command as further arguments. Stdin is 
  piped directly to the `docker container`.

```
    dumb-init -v -- docker run --rm -i             \
      -v /var/run/docker.sock:/var/run/docker.sock \
      -v $PWD:/home/dev                            \
      -v $PWORKSPACE/$DIST:/workspace              \
      -v $ARCHIVE:/archive                         \
      -u dev:dev                                   \
      -w "/home/dev"                               \
      $*
```

- `rootDocker` runs a `docker run` command with the standard `root:root` 
  user/group in the `/` working directory. Any command arguments are 
  appended to the `docker run` command as further arguments. Stdin is 
  piped directly to the `docker container`. 

```
    dumb-init -v -- docker run --rm -i             \
      -v /var/run/docker.sock:/var/run/docker.sock \
      -v $PWD:/home/dev                            \
      -v $PWORKSPACE/$DIST:/workspace              \
      -v $ARCHIVE:/archive                         \
      $*
```
