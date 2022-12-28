---
layout: post
title: Guide for using remote server (linux) for a data scientist!
date: 2022-05-20T23:00:00.000+00:00
description: This is a guide for people who want to use a remote linux server for data science stuff.
comments: true
tags: linux
---

Well, you have got a server (most probably linux) which you want to run your tasks on it. How should you do that?
Here I list all the challenges I've encountered.


## SSH

First of all, you know that you should connect to server via `SSH`: (1914 is just a dummpy number for port. Just select something that linux hasn't used)

    ssh -L 1914:localhost:1914 pourmand@ServerAddress

After that you will need to enter your password for the user `pourmand` in the server. 

> You may also want the server to **know** your computer i.e. it does not ask for password every time you want to connect to it.
> If that's the case, I suggest that you read [this](https://linuxhandbook.com/add-ssh-public-key-to-server/) tutorial. To put it in the nutshell, you sould copy your public ssh key to the server using `ssh-copy-id` command. 

In addition, you can also define your ssh servers in a file called `config`. This way you can connect to the server just by typing:

```bash
ssh my_server
```

For that, open `~/.ssh/config` file and write the following configuration:

```bash
HOST <<your_desired_host_name>>
	HOSTNAME <<hostname_ip>>
	USER <<your_username>>
	LOCALFORWARD 1717 localhost:1717
```

You can also set `tmux` to open automatically by adding the following lines ([Reference](https://stackoverflow.com/questions/27613209/how-to-automatically-start-tmux-on-ssh-session)) :

```bash
    RequestTTY yes 
    RemoteCommand tmux new -A -s <<your_tmux_session_name>>
```

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

## Nvidia-htop

You can see GPU information using `nvidia-smi`. But most of the time, it doesn't tell you all the information you need at once. You want to undestand who is using GPU or how much time GPU is being used and what not. You can see all you need using `nvidia-htop`. For installation, it is indeed very easy. 

```bash
pip install nvidia-htop

# then run
nvidia-htop.py
```

Usually, I use this command most of the time. So, I have a created an alias for it.

```bash
echo "alias nv='nvidia-htop.py'" >> ~/.profile

# use this if you do not already have .profile in your .bashrc and .zshrc
# this is very common
echo "source ~/.profile" >> ~/.bashrc
echo "source ~/.profile" >> ~/.zshrc
```

{% include figure.html path="assets/posts/remote_server/nvidia-htop.png" class="img-fluid rounded z-depth-1" zoomable=true %}

## NVITop

This one is very similar with `nvidia-htop` but it shows more! 

{% include figure.html path="assets/posts/remote_server/nvitop.png" class="img-fluid rounded z-depth-1" zoomable=true %}

Installation is also very easy:
```bash
pip install nvitop

# then run
nvitop
```