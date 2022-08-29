---
title: "FAIL: Why you should ALWAYS use visudo"
date: "2009-07-06"
categories: 
  - "technology"
tags: 
  - "fail"
  - "linux"
  - "sudo"
  - "work"
hide:
  - toc
---

:octicons-calendar-24: July 6, 2009

It's time for a short rant about proper Linux administration. Someone, who shall not be named, manually edited the /etc/sudoers file and broke it on a critical server. In case you don't know, on Linux sudo allows you to run commands as the root (Administrator) user, and the sudoers file determines who can use sudo and what they can do with it.

When editing the sudoers file you have to be careful, if you make a change that breaks the file you can potentially lock yourself out of being able to fix the file. That's what happened today, after the change to the sudoers file any attempt to use sudo returned this error:

```
>>> sudoers file: syntax error, line 6 <<<
>>> sudoers file: syntax error, line 8 <<<
sudo: parse error in /etc/sudoers near line 6
```

On this particular system users can only gain root access by using sudo, there is a root password but for some unfathomable reason no one knows the password. So I was tasked with rebooting the server, breaking into it and then fixing the sudoers file. Normally on Debian there's a helpful boot option that takes you right into a command prompt as root, unless of course you have a password set and don't know the password. So I resorted to the less elegant method of specifying init=/bin/bash as a boot option.

Once I had root access I used visudo to fix the sudoers file. Why visudo? Because that's the program **YOU ARE SUPPOSED TO USE**. Yes I am a bit angry, because the person that broke this server **should have known better**. Visudo is a lovely little program that checks the syntax of the sudoers file before you save it so that if you do something monumentally stupid you'll know about it before it becomes a problem and prevents you from getting back into the system as root.

Later this week I'll be posting an article with my list of good Linux administration habbits you'll want to ingrain into your skull.

So far the week is off to a great start.
