---
date: 2016-04-12
categories: 
    - "technology"
    - "tutorials"
tags: 
    - "automation"
    - "linux"
    - "raspberry-pi"
    - "zwave"
---

# Compiling open-zwave-control-panel on a Raspberry Pi 3

I've recently started diving into home automation stuff, and of course immediately ran into issues. I purchased a z-wave water sensor and planned to use [Home Assistant](https://home-assistant.io) to monitor the sensor and send me alerts (via pushover) if it detects water. While getting HA installed was quick and easy, the trouble began when I tried to actually read from the sensor. I could see in the _OZW\_Log.txt_ log file that the system was receiving data, but the HA log had no event information.

<!-- more -->

After a bunch of searching, I came across a post talking about using open-zwave-control-panel (ozwcp) to diagnose and configure z-wave devices. Awesome! Sadly, the documentation for this project is lacking, and it took a bit of trial and error to figure out the right steps to get ozwcp build, let alone run. Eventually I figured it out, reconfigured my sensor and everything worked. So to save others the frustration of trying to build ozwcp, I've got a quick and simple tutorial for you!

!!! tip "Update"

    There's a docker image that's only slightly out of date, I suggest trying that first _before_ going to the effort of compiling ozwcp. If you don't have docker installed, follow one of the many tutorials out there, then run this command (where /dev/ttyUSB0 is your zwave controller):

    ``` shell
    docker run -p 8008:8008 --device /dev/ttyUSB0 openzwave/openzwave-control-panel &>/dev/null &
    ```

And now back to our regularly scheduled programming.

Please note, these instructions have only been tested on Raspbian 8 (jessie) on a Raspberry Pi 3. Your milage may vary! All commands are run as _root_, because I'm lazy.

First, install some (probably) important libraries:

``` shell
apt-get update
apt-get install libgnutls28-dev libgnutlsxx28
```

Download, build and install libmicrohttpd

``` shell
cd
wget ftp://ftp.gnu.org/gnu/libmicrohttpd/libmicrohttpd-0.9.19.tar.gz
tar zxvf libmicrohttpd-0.9.19.tar.gz
mv libmicrohttpd-0.9.19 libmicrohttpd
cd libmicrohttpd
./configure
make
make install
```

Download and build the open-zwave library

``` shell
cd
git clone https://github.com/OpenZWave/open-zwave.git
cd open-zwave
make
```

Download open-zwave-control-panel

``` shell
cd
git clone https://github.com/OpenZWave/open-zwave-control-panel.git
cd open-zwave-control-panel
```

Open the _Makefile_ and find the following line:

``` makefile
OPENZWAVE := ../
LIBMICROHTTPD := -lmicrohttpd
```

Change it to:

``` makefile
OPENZWAVE := ../open-zwave
LIBMICROHTTPD := /usr/local/lib/libmicrohttpd.a
```

Build ozwcp

``` shell
make
ln -sd ../open-zwave/config
```

And finally, run it!

``` shell
./ozwcp -p 8888
```

Now you can open `http://your\_system:8888` in a browser to start using the open-zwave-control-panel!
