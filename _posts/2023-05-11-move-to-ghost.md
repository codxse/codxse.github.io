---
layout: post
title:  "Blogging with a Ghost platform"
date:   2023-04-25 19:36:16 +0700
categories: blogging
---
In search of a distraction-free blogging experience, I found myself reflecting on my early days with WordPress. It was once a simple platform that made writing a breeze. However, over time, I grew increasingly uncomfortable with WordPress as it evolved and changed.

The problem lies in the platform's plugin system. Designed to make a writer's life easier, it instead became a source of frustration. The moment I tried to customize my site, I was flooded with paid plugins, even for basic tasks like inserting Google Analytics tags.

Free plugin options, on the other hand, often come with their own set of issues: lack of updates, poor maintenance, and loaded with ads. It felt like my once-peaceful blogging space had turned into a chaotic marketplace.

That's when I discovered Ghost, a platform that promised a clean, distraction-free writing experience. Intrigued by its potential, I decided to give Ghost a try and began the installation process. To my surprise, it was straightforward, with only a few minor hiccups along the way.

## What need to be prepared

When it comes to trying out the Ghost platform, you've got two options: opt for Ghost hosting or install it on your own infrastructure.

But,.. unlike WordPress, which offers a free option for hosting on their platform, Ghost doesn't have a similar offering.

I chose to install Ghost on my own server, following [their tutorial](https://ghost.org/docs/install/ubuntu/?ref=zenness.co) which, luckily, isn't too complex. My journey began with [DigitalOcean](https://m.do.co/c/1d1638701633?ref=zenness.co).

<a href="https://www.digitalocean.com/?refcode=1d1638701633&amp;utm_campaign=Referral_Invite&amp;utm_medium=Referral_Program&amp;utm_source=badge"><img src="https://web-platforms.sfo2.cdn.digitaloceanspaces.com/WWW/Badge%201.svg" alt="DigitalOcean Referral Badge" loading="lazy"></a>

*Sponsored message: If you sign up within that link, you will get $100 credit.*

Initially, I tried installing Ghost on their most affordable droplet, priced at $4. However, I soon faced an out-of-memory error and realized I couldn't install MySQL with just 512MB of RAM.

Here's the tutorial I followed to install MySQL on the server

```sh
mariadb.service - MariaDB 10.5.18 database server
     Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; vendor preset: enabled)
     Active: failed (Result: exit-code) since Tue 2023-04-25 14:49:45 UTC; 19s ago
       Docs: man:mariadbd(8)
             https://mariadb.com/kb/en/library/systemd/
    Process: 32891 ExecStartPre=/usr/bin/install -m 755 -o mysql -g root -d /var/run/mysqld (code=exited, status=0/SUCCESS)
    Process: 32892 ExecStartPre=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
    Process: 32894 ExecStartPre=/bin/sh -c [ ! -e /usr/bin/galera_recovery ] && VAR= ||   VAR=`cd /usr/bin/..; /usr/bin/galera_recovery`; [ $? -eq 0 ]   && systemctl set-environment _WSREP_START_POSITION=$VAR |>
    Process: 32944 ExecStart=/usr/sbin/mariadbd $MYSQLD_OPTS $_WSREP_NEW_CLUSTER $_WSREP_START_POSITION (code=exited, status=1/FAILURE)
   Main PID: 32944 (code=exited, status=1/FAILURE)
     Status: "MariaDB server is down
```

But, things didn't go as smoothly as I'd hoped. I decided to upgrade my droplet, but the problem persisted. Every time I tried to restart, I encountered the same error:

```sh
Failed to start MariaDB 10.5.18 database server
```

In the end, I discovered that I needed to completely reinstall the MySQL server:

```sh
sudo apt remove --purge mysql-\* mariadb-\*
sudo apt install mariadb-server
```

The next step involved installing nginx, nodejs, and Ghost CLI. Fortunately, this part of the process went smoothly, and I didn't encounter any errors. And now, here we are – this is the Ghost blog you're currently reading!

So that's my short journey to setting up my very own Ghost blog.

Now, I'd love to hear from you. What are your experiences with blogging platforms like Ghost or WordPress? Have you encountered any challenges, or do you have any tips to share? Let's get the conversation started – leave your thoughts in the comments section below!