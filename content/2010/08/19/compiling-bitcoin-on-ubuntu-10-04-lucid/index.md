---
title: "Compiling Bitcoin on Ubuntu 10.04 (Lucid)"
date: "2010-08-19"
categories: 
  - "technology"
  - "tutorials"
tags: 
  - "bitcoin"
  - "cryptography"
  - "linux"
  - "sourceforge"
  - "ubuntu"
hide:
  - toc
---

:octicons-calendar-24: August 19, 2010

In case you don't know, [Bitcoin](http://bitcoin.org) is a decentralized crypto-currency or as the site says a "peer-to-peer network based digital currency". While most people have been content to use the pre-compiled binaries available, some of us prefer to compile the client from source, for various reasons. Personally, I maintain [a repository](http://github.com/aceat64/bitcoin-patchwork) on github because I like using a number of patches that various programmers have put forth in the Bitcoin community. Whatever your reason for compiling from source, you have likely run into issues with compiling the graphical client since it requires the development version of [wxWidgets](http://www.wxwidgets.org/). After a whole lot of failed attempts to compile the Bitcoin client I finally found a working configuration.

I have tested this process only on Ubuntu 10.04 (Lucid), both 64 and 32bit. From what I can tell earlier versions of Ubuntu and Debian should work just fine, but with a different set of packages. Specifically the "libboost-all-dev" package appears to only be available in Ubuntu 10.04, but the "libboost-dev" package may work for earlier versions and/or Debian.

First you need to install the proper packages:

``` shell
sudo apt-get install build-essential libgtk2.0-dev libssl-dev libdb4.7-dev libdb4.7++-dev libboost-all-dev checkinstall subversion git-core
```

Next, you will need to download the 2.9 version of wxWidgets from their SourceForge project. I suggest creating a folder in your home directory called "src", where we can compile wxWidgets and Bitcoin. Once the folder has been created we can download the [2.9 version of wxWidgets](http://sourceforge.net/projects/wxwindows/files/wxAll/2.9.0/wxWidgets-2.9.0.tar.gz/download) from their SourceForge project, and extract it. I've written out example steps below:

``` shell
cd ~
mkdir src
cd src
wget "http://downloads.sourceforge.net/project/wxwindows/wxAll/2.9.0/wxWidgets-2.9.0.tar.gz?r=&ts=1282200132&mirror=surfnet"
tar -zxvf wxWidgets-2.9.0.tar.gz
cd wxWidgets-2.9.0/
```

Now that wxWidgets is extracted, you are ready to begin compiling it. First, create a build directory and change to it.

``` shell
mkdir buildgtk
cd buildgtk
```

Now configure and build wxWidgets:

``` shell
../configure --with-gtk --enable-debug --disable-shared --enable-monolithic
make
```

We could just run "sudo make install", but that would put a bunch of wxWidget files all throughout your filesystem with no way to easily remove them. Luckily, the checkinstall program allows you to create a Debian package on the fly! This allows you to quickly and easily remove the files installed by wxWidgets. Let's get started:

``` shell
sudo checkinstall
```

You'll probably be prompted to "create a default set of package docs", just go ahead and hit enter. When it prompts you for the package description, just put "wxwidgets" without the quotes and hit enter twice. You should have a screen that looks like this:

```
*****************************************
**** Debian package creation selected ***
*****************************************
This package will be built according to these values:
0 -  Maintainer: [ root@ubuntu ]
1 -  Summary: [ wxwidgets ]
2 -  Name:    [ build ]
3 -  Version: [  ]
4 -  Release: [ 1 ]
5 -  License: [ GPL ]
6 -  Group:   [ checkinstall ]
7 -  Architecture: [ i386 ]
8 -  Source location: [ build ]
9 -  Alternate source location: [  ]
10 - Requires: [  ]
11 - Provides: [ build ]
Enter a number to change any of them or press ENTER to continue:
```

Select "3" for version, then put "2.9" as the version number. Once you are back at the package creation menu, hit enter again to install wxWidgets and build the package. Once it's done, you will need to run the following command:

``` shell
sudo ldconfig
```

Now you are ready to download the Bitcoin source and compile the client! Change back to the "src" directory we created earlier:

``` shell
cd ~/src
```

If you want to download the latest version of Bitcoin from the official subversion repository, run the following:

``` shell
svn co https://bitcoin.svn.sourceforge.net/svnroot/bitcoin/trunk bitcoin-trunk
```

Optionally you can download the source code for a different version of the Bitcoin client, such as from my git repository (with various community patches I've added):

``` shell
git clone git://github.com/aceat64/bitcoin-patchwork.git
```

Once the source code has been downloaded you will need to change to the directory of whichever version you picked. Once there you only need to run one command to compile the Bitcoin graphical client:

``` shell
make -f makefile.unix bitcoin
```

If you want to compile the bitcoin daemon, the command is:

``` shell
make -f makefile.unix bitcoind
```

If everything went well there will be a file called "bitcoin" in the current directory. I suggest copying the file to "/usr/local/bin" and making sure it's owned as root with the permissions 755 so that no one but root can modify it.

If this article helped you out, please send some Bitcoins to the [Bitcoin Faucet](https://freebitcoins.appspot.com/) and help spread the word about Bitcoin.
