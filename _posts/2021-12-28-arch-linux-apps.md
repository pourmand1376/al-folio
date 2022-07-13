---
layout: post
title:  Things I normally install after manjaro 
date: 2023-01-10T23:00:00.000+00:00
description:  
comments: true
tags: linux
---

Here, I keep my script for installing apps I need on arch linux, particularly manjaro kde.

As I like to explore softwares, I have usually search a lot for each one and tried to choose the best that fits my needs. Your's may be different. But since you have visited my website, it means that maybe you are a data scientist (or researcher) or you want to be one, so It is highly likely that you like this list!

I usually update my package manager called `pacman`:

    sudo pacman -Syu

Then I would certainly install `aura` package manager since it gives me an easy way to install `aur` packages. As you may know, 
packages in arch are basically found in offical arch packages or aur packages. I use `AUR` for both because it gives me a covenient way
to install any package.

    git clone https://aur.archlinux.org/aura-bin.git
    cd aura-bin
    makepkg
    sudo pacman -U <the-package-file-that-makepkg-produces>

That's it. Now the gate to installing anything is open. By anything, I mean really anything. You don't need to search `how to install blah blah blah`. 
Just use

    aura -Asr package_name

and it will show you the exact name to install the package you want.

> Some package names are ended with `bin` it means that aura will download prebuilt binary package and install it directly. Otherwise, it will most
> probably built the package on your system from scratch. Don't worry, you dont't have to do anything. Aura will take care of everything. 


