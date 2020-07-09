# Laminar scripts

This repository contains a collection of 
[Laminar](https://laminar.ohwg.net/) scripts and jobs. It also, 
incedentally, provides an example of one developer's use of Laminar. 

[Laminar](https://github.com/ohwgiles/laminar)
is an ultra simple and ultra light-weight 
[Continuous-Integration](https://en.wikipedia.org/wiki/Continuous_integration) 
/ [Continuous-Delivery](https://en.wikipedia.org/wiki/Continuous_delivery) 
tool which I am using on my own development machine to help me develop a 
number of projects. 

For my needs, I only really use Laminar in a "semi-continuous" mode. When 
ever I have major changes I fire up Laminar and start the overall build 
process for a particular project. To do this, I have written a small 
collection of bash scripts. You can find these scripts in the `bin` directory.

On my development machine, I have [Podman](https://podman.io/) installed 
so I use podman containers to control the build / test / deploy 
environment. So one of the most important Laminar jobs I require, is a job 
to (re)build the basic podman images I use. You can find this job in the 
`jobs` directory. 

This repository contains a (small) collection of scripts and Laminar jobs. 
It also contains some Tips and Tricks documents to remind myself how and 
why I have done things in the way they are. 

Hopefully others will find these Tips and Tricks as well as the associated 
example Laminar jobs useful. (Your milage may vary).

I have captured my project specific Laminar jobs in the following sister 
repositories: 

  - [pdf2htmlEX-laminar-scripts](https://github.com/stephengaito/pdf2htmlex-laminar-scripts) 

## Installation

I have decided to build Laminar from its sources *and* install it into a
`laminar` directory in my `dev` user's home directory. 

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

Git clone laminar:

```
  git clone https://github.com/ohwgiles/laminar.git
  mkdir -p laminar/build
  cd laminar
```

Note that we specifically build and install locally into the dev user's 
laminar directory. 

Update laminar/CMakeLists.txt file:

```
--- CMakeLists.txt.orig	2020-07-07 09:12:02.762069806 +0100
+++ CMakeLists.txt	2020-07-07 09:19:48.648156986 +0100
@@ -133,9 +133,9 @@
 set(ZSH_COMPLETIONS_DIR /usr/share/zsh/site-functions CACHE PATH "Path to zsh completions directory")
 install(TARGETS laminard RUNTIME DESTINATION sbin)
 install(TARGETS laminarc RUNTIME DESTINATION bin)
-install(FILES etc/laminar.conf DESTINATION /etc)
-install(FILES etc/laminarc-completion.bash DESTINATION ${BASH_COMPLETIONS_DIR} RENAME laminarc)
-install(FILES etc/laminarc-completion.zsh DESTINATION ${ZSH_COMPLETIONS_DIR} RENAME _laminarc)
+install(FILES etc/laminar.conf DESTINATION ${CMAKE_INSTALL_PREFIX}/etc)
+install(FILES etc/laminarc-completion.bash DESTINATION ${CMAKE_INSTALL_PREFIX}/${BASH_COMPLETIONS_DIR} RENAME laminarc)
+install(FILES etc/laminarc-completion.zsh DESTINATION ${CMAKE_INSTALL_PREFIX}/${ZSH_COMPLETIONS_DIR} RENAME _laminarc)
 
 configure_file(etc/laminar.service.in laminar.service @ONLY)
-install(FILES ${CMAKE_CURRENT_BINARY_DIR}/laminar.service DESTINATION ${SYSTEMD_UNITDIR})
+install(FILES ${CMAKE_CURRENT_BINARY_DIR}/laminar.service DESTINATION ${CMAKE_INSTALL_PREFIX}/${SYSTEMD_UNITDIR})
```

Then build:

```
  cd build
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
