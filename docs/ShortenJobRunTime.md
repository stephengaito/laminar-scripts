# Ways to shorten the run-time of a Laminar job

Outside of raw compilation (which *is* what I am (re)testing) my Laminar 
jobs spend considerable time `git clone`ing large projects as well as 
installing required Debian archives. 

So two ways of shortening the run-time of my jobs is to:

1. Install apt-cacher-ng and add:

```
  echo 'Acquire::http { Proxy "http://<<yourServerIP>>:3142"; };' > /etc/apt/apt.conf.d/00aptcacher && \
```

*before* any apt commands in your `Dockerfile`/`Containerfile`s.

2. Rewrite any `git clone`s in your job scripts to add:

```
  --depth=50 --branch=$BRANCH
```

to ensure ONLY the last "50" commits on the branch you need are cloned.

