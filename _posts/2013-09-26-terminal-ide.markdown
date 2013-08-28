---
layout: post
title:  "Terminal IDE"
date:  2013-09-26
categories: [setup git ssh jekyll]
---
# Terminal IDE #

Many friends of mine root their Android devices to get them to act more like a GNU/Linux system. I've never been a big fan of this as it leaves most of the less technical novices or devices that are still locked out in the cold. Because of this, I use non priviledged software whenever I can. 

I've been using git for far more things than distributed source control and this journey started out with looking for a good git client for Android. My first search gave two results: a nonfree java port called Agit, for which I'm reluctant to trust it's correctness, the other is Terminal IDE. Terminal IDE had a fairly long list of other features that I didn't need but were welcome: a bash shell, ssh, vim, java compiler, and long list of GNU tools. It seemed to be everything I was looking for... that is until I tried to use the package.

The good news is Terminal IDE is fully functional. The bad news is Android makes it really awkward to use and some things requires scripts to get around differences from an Android System and a GNU/Linux system. My first task in Terminal IDE was to get git working with github where I immediately ran into problems.

## Setting up SSH ##

I started by generating an ssh key. So I type ssh-keygen and oh wait that's not installed. Instead the author choose dropbear for ssh so keys were created with

{% highlight bash %}
mkdir ~/.ssh
dropbearkey -t rsa -f ~/.ssh/id_rsa
{% endhighlight %}

This doesn't put the public key in a file. It just prints it to standard out. To get the private key into a file

{% highlight bash %}
dropbearkey -t rsa -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.pub
{% endhighlight %}

The finger print was also copied to the file but instead of scripting the removal of it I just deleted it in vim. Now the public key is ready to be added to a github account. Getting it into a github account is a little annoying since Terminal IDE doesn't support copy and paste. What I did was save the key in a file on the device's SDCARD then I opened the file in an android based editor and copied the text from there. Not ideal but functional. So from here ssh for git should work right?

## Hooking Git into Dropbear ##

{% highlight bash %}
git clone git@github.com:ttricker/ttricker.github.io
cloning into 'ttricker.github.io' ...
ssh: connection to git@github.com:22 exited no auth methods could be used
{% endhighlight %}

Well balls looks like git needs to be setup as well. The problem here is the arguments to dropbear are different than openssh. Now a script is needed to setup dropbear arguments for git and the GIT_SSH environment variable needs to be set.

{% highlight bash %}
vim ~/.ssh/sssh
~~~~~~~~~~~~~~ .sssh file begin
#!/data/data/com.spartacusrex.spartacuside/files/system/bin/bash
ssh -i ~/.ssh/id_rsa $*
~~~~~~~~~~~~~~ .sssh file end
chmod 755 ~/.ssh/sssh

vim ~/.bashrc
~~~~~~~~~~~~~~ add to bashrc start
export GIT_SSH=~/.ssh/sssh
~~~~~~~~~~~~~~ add to bashrc end
{% endhighlight %}

Now just restart Terminal IDE and git works.

Aside: Permissions are interesting in Android; on my phone, a Nexus 4, the SD card is writable and readable but permissions cannot be changed. That means the execution bit on a script cannot be set on the sdcard. 

