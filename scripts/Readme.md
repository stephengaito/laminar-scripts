# General Laminar scripts

This directory provides a collection of general [Laminar (shell) 
scripts](https://laminar.ohwg.net/docs.html#Helper-scripts) which are 
usable in any job. 

## `setupLaminarVars` bash functions

The main `setupLaminarVars` script contains the following bash functions:

- `recordVar` records an environment variable into the `LAMINAR_VARS` file 
  for later (re) sourcing in later scripts. 

- `recordComment` records a comment into the `LAMINAR_VARS` file for later 
  (re) sourcing in later scripts. 

- `failResult` runs an other bash script and exits the script if the 
  sub-bash script fails. It records any FAILUREs in the `LRESULTS_FILE` 
  file. 

- `acceptResult` runs an other bash script and records its success or 
  failure in a results file for later reporting (and potential failure). 
  It records any SUCCESSes or FAILUREs in the `LRESULTS_FILE` file. 

- `reportResults` reports the LRESULTS_FILE and exists with an non-zero 
  error code if any result failed. 

- `basePodman` runs a `podman run` command in a specified podman container 
  with no changes in user or workDir. Any command arguments are appended 
  to the `podman run` command as further arguments. Stdin is piped 
  directly to the `podman container`. 

```
    podman run --rm                  \
      -v /etc/timezone:/etc/timezone \
      -v $LAMINAR_HOME:$LAMINAR_HOME \
      $*

```

- `devPodman` runs a `podman run` command with an internal `dev:dev` 
  user/group in the `/home/dev` working directory. Any command arguments 
  are appended to the `podman run` command as further arguments. Stdin is 
  piped directly to the `podman container`.

```
    podman run --rm -i                             \
      -v $PWD:/home/dev                            \
      -v $PWORKSPACE/$DIST:/workspace              \
      -v $ARCHIVE:/archive                         \
      -u dev:dev                                   \
      -w "/home/dev"                               \
      $*
```

- `rootPodman` runs a `podman run` command with the standard `root:root` 
  user/group in the `/` working directory. Any command arguments are 
  appended to the `podman run` command as further arguments. Stdin is 
  piped directly to the `podman container`. 

```
    podman run --rm -i                             \
      -v $PWD:/home/dev                            \
      -v $PWORKSPACE/$DIST:/workspace              \
      -v $ARCHIVE:/archive                         \
      $*
```

## `setupLaminarVars` environment variables

The main `setupLaminarVars` script contains the following bash functions:

- `LAMINAR_RUNDIR` is the initial `$(pwd)` directory saved for later 
  re-use. 

- `LRESULTS_FILE` is the name of the file used to record `acceptResult` or 
  `failResult` results. 

- `LAMINAR_VARS` is the name of the file used to record environment 
  variables for (re) sourcing in later scripts. 

- `BASE_PODMAN` an environment variable version of the `basePodman` bash 
  function. 

- `ROOT_PODMAN` an environment variable version of the `rootPodman` bash
  function.

- `DEV_PODMAN` an environment variable version of the `devPodman` bash 
  function. 

