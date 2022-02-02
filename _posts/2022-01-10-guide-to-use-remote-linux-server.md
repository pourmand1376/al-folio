---
layout: post
title: A Guide to use remote linux server
date: 2021-12-28T23:00:00.000+00:00
description: This is a guide for people who want to use a remote linux server for data science stuff.
comments: true
---

Well, you have got a server (most probably linux) which you want to run your tasks on it. How should you do that?
Here I list all the challenges I've encountered.

First of all, you know that you should connect to server via `SSH`: (1914 is just a dummpy number. Just select something that linux hasn't used)

    ssh -L 1914:localhost:1914 pourmand@ServerAddress

> You may also want the server to **know** your computer i.e. it does not ask for password every time you want to connect to it.
> If that's the case, I suggest that you read [this](https://linuxhandbook.com/add-ssh-public-key-to-server/) tutorial.

