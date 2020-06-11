# Laminar scripts

This repository contains a collection of 
[Laminar](https://laminar.ohwg.net/) scripts and jobs. It also, 
incedentally provides an example of one developer's use of Laminar. 

[Laminar](https://github.com/ohwgiles/laminar)
is an ultra simple and ultra light-weight 
[Continuous-Integration](https://en.wikipedia.org/wiki/Continuous_integration) 
/ [Continuous-Delivery](https://en.wikipedia.org/wiki/Continuous_delivery) 
tool which I am using on my own development machine to help me develop a 
number of projects. 

For my needs, I only really use Laminar in a "semi-continuous" mode. When 
ever I have major changes I fire up Laminar and start the overall build 
process for a particular project. To do this, I have written a small 
collection of bash scripts. 

On my development machine, I already have Docker installed so I use docker 
images to control the build / test / deploy environment. So one of the 
most important Laminar jobs I require, is a job to (re)build the basic 
docker images I use. 

This repository contains a (small) collection of scripts and Laminar jobs. 
It also contains some Tips and Tricks documents (to remind myself how and 
why I have done things in the way they are). 

Hopefully others will find these Tips and Tricks as well as the associated 
example Laminar jobs useful. (Your milage may vary).

I have captured my project specific Laminar jobs in the following sister 
repositories: 

  - [pdf2htmlEX-laminar-scripts](https://github.com/stephengaito/pdf2htmlex-laminar-scripts) 

## Installation

I have decided to build Laminar from its sources *and* install it into a
`laminar` directory in my `dev` user's home directory. 

Since I make extensive use of docker, *and* my Chief Security Officer (me) 
mandates that docker is used with the namespaces option *ON*, I have 
created a `dev` user on my machine whose unix user and group ids match 
those inside docker containers. This allows me to share files seamlessly. 

### [Building Laminar](https://github.com/ohwgiles/laminar#building-from-source)

Install the required dependencies:

```
  sudo apt install \
    capnproto      \
    cmake          \
    g++            \
    libboost-dev   \
    libcapnp-dev   \
    libsqlite-dev  \
    libsqlite3-dev \
    make           \
    rapidjson-dev  \
    zlib1g-dev
```

Then build:

```
  git clone https://github.com/ohwgiles/laminar.git
  mkdir -p laminar/build
  cd laminar/build
  cmake                                          \
        -DCMAKE_BUILD_TYPE=Release               \
        -DCMAKE_INSTALL_PREFIX=/home/dev/laminar \
        ..
  make -j2
  make install
```

### Installing laminar-scripts

Type:

```
  git clone https://github.com/stephengaito/laminar-scripts.git
  cd laminar-scripts
  cp laminar.conf.example laminar.conf
```

Then update the environment variables in the laminar.conf file to suit 
your needs.

Then type:

```
  ./setup
```
