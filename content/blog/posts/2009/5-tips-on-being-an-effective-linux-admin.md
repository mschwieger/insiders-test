---
date: 2009-07-21
categories: 
  - "technology"
tags: 
  - "apache"
  - "google"
  - "linux"
  - "raid"
  - "sudo"
  - "work"
---

# 5 Tips On Being An Effective Linux Admin

Earlier this week someone [did something very bad](http://lickthesalt.com/2009/07/06/fail-why-you-should-always-use-visudo/), and it got me thinking about what are some of the simple things that make someone an effective Linux admin. While I will not tell you that following my advice will make you some kind of super-admin, it will help you when dealing with Linux servers and hopefully save you and others from needless headaches. Here are my tips on being an effective Linux admin, in no particular order.

<!-- more -->

## No1. Backup Files Before Modifying Them

Even if you think you are some kind of Linux god, you should always make backups of critical configuration files. In my time as a Linux admin, I have seen some stupid things, but the dumbest by far is when someone edits an incredibly complex config file and does not back it up first. Oh but you are just making a quick change right, maybe just modifying one line? It does not matter, because eventually you will make a mistake and you will be glad you had the foresight to make that backup first.

Personally, I make a copy of the file in question and append my initials to the end, so "apache2.conf" becomes "apache2.conf.asl". The beauty of this method is that other users will know who changed a file and when, because they can simply look at the modify time for the file. Genius, I know.

``` shell
cp apache2.conf apache2.conf.asl
```

It is that easy! There is no reason to skip this step before modifying your configuration files.

## No2. Comment, Comment, Comment

I am somewhat OCD when it comes to commenting. I comment religiously for a reason: I have a terrible memory. More than once I have been looking at something I have written or changed and wondered, WTF was I thinking. Commenting allows me to remember why I did something, or what it does. This is especially important when writing scripts or programs.

Comments are important when you are doing work on a customer's server. Every time I make a configuration change for a customer, I put in a comment with the date, why I am changing the setting, the ticket ID# for the issue and my initials. This makes it easy for the customer or a technician to find information about the particular change made if there is ever an issue with it in the future.

## No3. Documentation

Where I work, we have some older servers that were setup by "The UNIX God". This particular individual was given that title by someone in the company that had almost no \*NIX knowledge. He left his mark on our systems in a number of ways that we are still paying the price for, so I have a number of colorful names that I use to refer to him.

Documentation is important. Imagine the following situation, it is 3am, your boss calls you in a panic, a mission critical server is down, he wants you up at the datacenter or office to take care of it right now. When you get to the datacenter, you find out the server is not down, it is dead, and I am talking power supply caught fire and killed every component. The motherboard is toast, the raid array you watched slowly build is completely shot. No worries, this is why you have backups. You will just have to load an OS on a new box, load up the right services and then restore the data.

However, there is a problem; you do not know all the details of this server's configuration. You backed up the data and the config files, but you do not have a full image of the OS. You have no idea what specific packages, utilities and libraries your company's custom written programs require. If you documented the steps taken to install the OS and every service when the system was first setup, you are golden; you just open up the documentation and retrace your steps. If you do not have documentation, you are looking forward to a long sleepless night of setting up services and then wadding through error messages to figure out what library you have overlooked.

Having good documentation is a lot easier then most people realize, all that's really needed is a wiki. MediaWiki is easy to setup and easy to use, it is the software that powers Wikipedia. MediaWiki only needs Apache, PHP and MySQL, and has a huge support community and wiki with documentation. Grab a copy of [MediaWiki](http://www.mediawiki.org/wiki/MediaWiki) and get it installed.

Next, create a page in your new wiki for each server, project, or cluster you setup. It does not have to be terribly complex, some of mine are simply a list of commands I ran with some basic comments about what the more obscure ones do or why they are required.

Do not forget to keep it up-to-date! Whenever you change something big, or small, be sure you update the wiki to reflect the change.

## No4. Google

This one should be obvious; Google search is your friend. It is usually very rare that you will come across an issue that no one else in the world has ever run into before. Which means that there is a good chance that no matter what is broken, someone, somewhere, has already found out the solution to the problem.

Effectively finding what you need is hard for some people. Sometimes your search turns up useless results, nothing at all, or worse yet bad advice. Unless the answer is coming from an authoritative source, like the official _documentation_ (there is that word again), or from someone involved with the software, be wary. You should know not only _what_ the command or setting will do, but _why_ it will work. If you remember to make backups, you will be just fine, even if someone gives you bad advice.

## No5. Break Things (and then rebuild them)

I am not saying you should go out and break critical infrastructure, as fun as that sounds. What you should do is setup replicas of existing systems and see if you have all the information and tools necessary to rebuild the system. If you can, without cheating, setup a similar system, then the documentation is good and you now have experience in replacing the server if it should die. If for some reason you cannot recreate the system with the available information, then something is obviously missing from the documentation, find what is missing and properly document it.

Another fun exercise is to clone a server, and then have a friend break a configuration file or two. If you want to be thorough, have them reset all the timestamps in /etc so you cannot do something like this:

``` shell
find /etc -mtime -1
```

That will show you a list of files modified in the last day, it is an incredibly useful command if you know someone has been messing with files and you want to know which files.

That is all the advice I have for now. If you have any suggestions for an article, such as a tutorial for a specific piece of software or issue, please post a comment below.
