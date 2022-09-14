---
date: 2009-07-01
categories: 
  - "technology"
  - "tutorials"
tags: 
  - "firefox"
  - "linux"
  - "sourceforge"
  - "ubuntu"
---

# Firefox 3.5: Impatiently Waiting

In case you have been living under a rock, Firefox 3.5 came out today (well technically yesterday). I've noticed a lot of people wondering how they can get it running smoothly on Ubuntu. Never fear, after much trial and error (a lot of error) I've found what I think should be the easiest way to install the latest version of Firefox. So if you are just as impatient as I am, you can rejoice in the all the cool new features.

<!-- more -->

First up, I used a script called [Ubuntuzilla](http://ubuntuzilla.wiki.sourceforge.net/), it takes care of virtually all the dirty work. Personally, I like it because whenever Ubuntu gets around to adding Firefox 3.5 to the official repositories (I don't count [Minefield](http://www.mozilla.org/projects/minefield/))  you can simply use Ubuntuzilla to remove your "unofficial" version of Firefox and use Ubuntu's packaged version again.

Before you install Ubuntuzilla, we'll need to install some dependencies. So open up a terminal and run the following command, which will install the dependencies from Ubuntu's repository. If you don't know how to use the terminal, you probably want to stop and go read the wonderful guide at [linuxcommand.org](http://linuxcommand.org/) first.

``` shell
sudo apt-get install libstdc++5 libnotify-bin
```

Next, you'll need to know if you are running the 32bit or 64bit version of Ubuntu. The quickest way will be to run this in your terminal:

``` shell
uname -a
```

The part we're looking for is near the very end of the line. If it says "i686" you are using 32bit, if it says "x86\_64" you're using 64bit. Now that you know what version of Ubuntu you are running it's time to download and install Ubuntuzilla.

For 32bit, we run:

``` shell
wget http://softlayer.dl.sourceforge.net/sourceforge/ubuntuzilla/ubuntuzilla-4.6.1-0ubuntu1-i386.deb
sudo dpkg -i ubuntuzilla-4.6.1-0ubuntu1-i386.deb
```

For 64bit:

``` shell
wget http://softlayer.dl.sourceforge.net/sourceforge/ubuntuzilla/ubuntuzilla-4.6.1-0ubuntu1-amd64.deb
sudo dpkg -i ubuntuzilla-4.6.1-0ubuntu1-amd64.deb
```

Once it's installed, you just run this command and follow the prompts (it's pretty straightforward):

``` shell
sudo ubuntuzilla.py
```

If everything went smoothly, you can open up Firefox as normal and it will be version 3.5! Of course, if you're like me and you use subpixel smoothing (known as ClearType on Windows) the fonts in Firefox might look, well, ugly. Don't worry though, we can fix it!

NOTE: This step is COMPLETELY OPTIONAL, this is only for users who need subpixel smoothing for their fonts. We're going to create a file in our home directory called ".fonts.conf" and put in some configuration options to tell Firefox (and other programs that don't listen to Gnome's font settings) that we want smooth, beautiful fonts. From the terminal, run the following

``` shell
gedit ~/.fonts.conf
```

That will open up our text editor (gedit), now put this into the file, save it, and close gedit.

``` xml
<?xml version=”1.0″?>
<!DOCTYPE fontconfig SYSTEM “fonts.dtd”>
<fontconfig>
  <match target=”font” >
    <edit mode=”assign” name=”rgba” >
      <const>rgb</const>
    </edit>
  </match>
  <match target=”font” >
    <edit mode=”assign” name=”hinting” >
      <bool>true</bool>
    </edit>
  </match>
  <match target=”font” >
    <edit mode=”assign” name=”hintstyle” >
      <const>hintfull</const>
    </edit>
  </match>
  <match target=”font” >
    <edit mode=”assign” name=”antialias” >
      <bool>true</bool>
    </edit>
  </match>
</fontconfig>
```

Please note, these settings are specifically to enable full smoothing, I'm not sure what the settings would be for medium or slight smoothing. If anyone knows, please post in the comments and I'll update this post.

If you run into any trouble, drop me a line in the comments and I'll try to help you out. Now go have fun surfing the web :)

**Update 7/12:**

Thank you Antsu for providing the settings for slight and medium smoothing.

For slight smoothing:

``` xml
<match target=”font” >
  <edit mode=”assign” name=”hintstyle” >
    <const>hintslight</const>
  </edit>
</match>
```

and for medium smoothing:

``` xml
<match target=”font” >
  <edit mode=”assign” name=”hintstyle” >
    <const>hintmedium</const>
  </edit>
</match>
```
