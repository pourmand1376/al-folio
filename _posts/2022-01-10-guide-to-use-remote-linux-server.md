---
layout: post
title: Ultimate Guide for using remote server (linux) for a data scientist!
date: 2023-12-28T23:00:00.000+00:00
description: This is a guide for people who want to use a remote linux server for data science stuff.
comments: true
---

Well, you have got a server (most probably linux) which you want to run your tasks on it. How should you do that?
Here I list all the challenges I've encountered.

First of all, you know that you should connect to server via `SSH`: (1914 is just a dummpy number for port. Just select something that linux hasn't used)

    ssh -L 1914:localhost:1914 pourmand@ServerAddress

After than you will need to enter your password for the user `pourmand` in the server. 

> You may also want the server to **know** your computer i.e. it does not ask for password every time you want to connect to it.
> If that's the case, I suggest that you read [this](https://linuxhandbook.com/add-ssh-public-key-to-server/) tutorial.

Then you can run your commands on the server, e.g.:

    python train.py ...

This is especially good for commands that do not take long time to run. If you have a model that need to be trained for days or even longer, you can not use this. That's because the moment you close your `ssh`, all your normal processes would die. So, you will need to use a [terminal multiplexer](https://wiki.archlinux.org/title/List_of_applications#Terminal_multiplexers) like [tmux](https://github.com/tmux/tmux/wiki) or `screen` to handle this problem. Both of them are free and open source and both have easy commands to achieve your needs. You can read full pros and cons [here](https://superuser.com/questions/236158/tmux-vs-screen). I use `tmux` because the syntax is easier to use.


