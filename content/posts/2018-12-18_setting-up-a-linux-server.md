+++
title = "What I do when setting up a new linux server"  
date = "2018-12-18" 
lastmod = "2019-08-20"  
category = "tech"
tags = ["linux", "server"]
keywords = ["tech", "technology", "servers", "linux", "ubuntu", "debian"]    
+++

This post is a bit more techy. If that's not something that interests you, sorry...but this is my blog so I get to decide what content is on here. I thought I would write a post detailing the process that I go through when I set up a new linux server (either a remote server or re-installing my Raspberry Pi...which I will actually be doing very soon, probably in less than 6 hours).  


First of all, I use debian-based linux distros as it is what I am the most familiar with. I have never used CentOS, Fedora, or Arch. After logging in for the first time, as root, here's what I do:  

    apt-get update  
    apt-get install build-essential git weget nano ufw dnsutils curl fail2ban logwatch sshguard -y  
    apt-get upgrade -y  
    dpkg-reconfigure locales  
    dpkg-reconfigure tzdata  
    ufw allow 22/tcp  
    ufw allow 443/tcp  
    ufw allow 80/tcp  
    ufw enable  

At the minimum, this is the list of software that I make sure are installed. I also setup a firewall as step one of my security procedures. I then setup my non-root user account.  

     adduser joshua
     nano /etc/sudoers
     nano /etc/ssh/sshd_config

I give `sudo` access to the newly created user account. And then part 2 of my security procedures is to lock down `ssh`. I keep ssh on port 22, disable root login, and disable password login to login with an ssh key.  

Finally, I like to setup a crontab to keep an eye on things with logwatch.  

    @weekly /usr/sbin/logwatch --output mail --mailto email@example.com --detail high --range '-7 days'  

---
[This website](https://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers) and [this website](https://www.linode.com/docs/security/authentication/use-public-key-authentication-with-ssh/) have been extremely helpful in helping me setup new servers.  