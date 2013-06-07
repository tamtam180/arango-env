
# arango-env #

arango-env is simple version management for [ArangoDB](https://github.com/triAGENS/ArangoDB/) like a rvm.  
I have checked only CentOS 6.x

# Required #

`git, awk, sed, make, sleep, pidof, grep` and some library for compile ArangoDB.

# Install #

```bash
$ export ARANGO_ENV_HOME=$HOME/.arango-env

$ cd /path/to
$ git clone git://github.com/tamtam180/arango-env.git
$ source /path/to/arango-env/arango-env
```

`ARANGO_ENV_HOME` environment is base directory for this system.
source command register "arango-env" function.
Do not direct call arango-env from shell, you cannot "use" command (because cannot change PATH environemnt)

# Usage #

```
  arango-env [install|uninstall|use|default|list|port|info|start|stop|status|help]

    - install [version]     Install ArangoDB
                            set version to "master" if empty.
    - uninstall [version]   Uninstall ArangoDB
    - use [version]         Change version.
    - default [version]     Set default version.
    - list [-a]             Listup installed version.
                            -a option is listup ALL available version. (include not install).
    - port                  Change configure a port number of current version.
    - info                  Display current version and default version.
    - start                 Start arangod of current version.
    - stop                  Stop arangod of current version.
    - status                Display process status of current version.
    - help                  Display help.
```

## Usage Example ##

```
$ source /path/to/arango-env

$ arango-env info

 directory = /home/tamtam/.arango-env
 - default = 
 - current = 

$ arango-env install v1.3.1
$ arango-env install v1.3.0
$ arango-env install         # install master branch

$ arango-env default master  # set default version
$ arango-env use v1.3.1      # change current verson (and reconfigure PATH)

  * master
    v1.3.0
 => v1.3.1

# => - current
# =* - current && default
#  * - default

$ arango-env info

 directory = /home/tamtam/.arango-env
 - default = master
 - current = v1.3.0

$ which arangosh arangod
~/.arango-env/arangodbs/v1.3.1/bin/arangosh
~/.arango-env/arangodbs/v1.3.1/sbin/arangod

$ arango-env start           # start arangod by daemon mode. (not supervisor mode.)
Start success! pid=14880

$ arango-env status
process running. pid=14880

$ curl http://localhost:8529/_admin/version
{"server":"arango","version":"1.3.1"}

$ arango-env port 9999       # Change port 8529(default port) to 9999
                             # if arangod already start, auto restart.

Restart arangod!
process running. pid=14880
process running. pid=14880
...
process running. pid=14880
process is not running. pid=
Start success! pid=18859

$ curl http://localhost:9999/_admin/version
{"server":"arango","version":"1.3.1"}

$ arango-env stop
process running. pid=18859
process running. pid=18859
...
process running. pid=18859
process is not running. pid=
```

## install ##
## uninstall ##
## use ##
## default ##
## list ##
## port ##
## info ##
## start ##
## stop ##
## status ##
## help ##


# Directory Structure #

```
~/.arango-env               # $ARANGO_ENV_HOME
  + arangodbs               # install directories
    + master
    + devel
    + v...
    + v1.3.1
      + bin                 # PATH
      + etc
        + arangodb          # config directory
      + sbin                # PATH
      + share
      + tmp                 # --tmp-path
      + var
        + lib
        + log               # arangodb log
          + arangodb
        + run
          + arangod.pid
        + tmp               # --working-directory
  + git                     # git clone repogitory
```

# ArangoDB Build option #

```
configure
  --prefix="..." \
  --enable-all-in-one-libev \
  --enable-all-in-one-v8 \
  --enable-all-in-one-icu \
  --enable-mruby
```

make command with "-j" option.

```
make -j $(($(grep -c processor /proc/cpuinfo)+1)) > /dev/null
```

# TODO #

* add restart command
* multi status list view
    * display port, start/stop
    * save port to file.
* add tag args to start/stop/status command
* display build progress ( ... )
* when uninstall, stop if already start.

# Author #

[tamtam180](https://twitter.com/tamtam180) < kirscheless at gmail.com >

# License #

Apache License, Version 2.0


