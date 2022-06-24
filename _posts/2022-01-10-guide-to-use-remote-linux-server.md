---
layout: post
title: Ultimate Guide for using remote server (linux) for a data scientist!
date: 2022-05-20T23:00:00.000+00:00
description: This is a guide for people who want to use a remote linux server for data science stuff.
comments: true
---

Well, you have got a server (most probably linux) which you want to run your tasks on it. How should you do that?
Here I list all the challenges I've encountered.


## SSH

First of all, you know that you should connect to server via `SSH`: (1914 is just a dummpy number for port. Just select something that linux hasn't used)

    ssh -L 1914:localhost:1914 pourmand@ServerAddress

After that you will need to enter your password for the user `pourmand` in the server. 

> You may also want the server to **know** your computer i.e. it does not ask for password every time you want to connect to it.
> If that's the case, I suggest that you read [this](https://linuxhandbook.com/add-ssh-public-key-to-server/) tutorial. To put it in the nutshell, you sould copy your public ssh key to the server using `ssh-copy-id` command. 

## Tmux

Then you can run your commands on the server, e.g.:

    python train.py ...

This is especially good for commands that do not take long time to run. If you have a model that need to be trained for days or even longer, you can not use this. That's because the moment you close your `ssh`, all your normal processes would die. So, you will need to use a [terminal multiplexer](https://wiki.archlinux.org/title/List_of_applications#Terminal_multiplexers) like [tmux](https://github.com/tmux/tmux/wiki) or `screen` to handle this problem. Both of them are free and open source and both have easy commands to achieve your needs. You can read full pros and cons [here](https://superuser.com/questions/236158/tmux-vs-screen). I use `tmux` because the syntax is easier to use.

Here is a simple tmux session I have created. Note that the session has 6 open tabs. Each of them may do something different. 

{% include figure.html path="assets/posts/remote_server/tmux.png" class="img-fluid rounded z-depth-1" zoomable=true %}

> If you install tmux, out of the box, it won't look like this. You have to install [Oh-my-tmux](https://github.com/gpakosz/.tmux) to make it look pretty. It is pretty easy to install. Just copy the lines and you're set. 

## Bash, Zsh

Also, if you are using `bash`, I recommend installing [Oh-my-bash](https://github.com/ohmybash/oh-my-bash) and if you are working with `zsh`, I recommend installing [Oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh). Actually, we live in the terminal. It shouldn't look like 80's anymore. It should look pretty.

## Miniconda 

Then, you want to install python packages in your environment. But hold on. You can not and should not install them system-wide. This is because multiple projects would require different versions of packages. They may even require different python versions other than preinstalled ones. We need a solution to handle all of them. 

`Miniconda` in my opinion is a very good solution to all of these problems. You can install it using [this guide](https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html). Once installed, you basically need these commands. 

```bash
# create a new env
conda create -n venv

# create a new env with specified python version
conda create -n venv python=3.8

# to activate the env
conda activate venv
```

Now for package installation, you can use both `conda install` and `pip install`. Some people say that you shouldn't use both of them because it may cause problems. I just tend to use conda for creating and activating environment. Then, installing everything else with `pip` and that works just fine.

## Htop

I can't think anyone not using this tool. It shows you all processes and their memory usuage and what not. 


{% include figure.html path="assets/posts/remote_server/htop.png" class="img-fluid rounded z-depth-1" zoomable=true %}