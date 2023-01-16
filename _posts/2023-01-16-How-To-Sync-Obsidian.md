---
layout: post
title: How to Sync Obsidian between Android, Windows and Mac
date: 2023-01-16T00:00:00.000+00:00
description: How I use git to Sync my obsidian notes seamlessly between all operating systems?
comments: true
tags: obsidian
---

I've tried a lot of tools for syncing my notes and they all have their issues. Whether it's Google Drive, Dropbox, or any other cloud sync service, they all seem to have their problems, especially when you're constantly editing the same files. But, after trying different methods, I've found that the best way to keep my notes in sync is by using `git`.

Installing the obsidian-git plugin on desktop systems like Windows and Mac is a breeze. It works seamlessly and there's no additional setup required. If you're new to git, I recommend creating a new private GitHub repository to store your obsidian notes. Check out this link for more info: [GitHub - denolehov/obsidian-git: Backup your Obsidian.md vault with git](https://github.com/denolehov/obsidian-git)

The main issue I've found is when I want to sync my notes with my Android device. I don't want to constantly deal with git merge conflicts. The solution to this is automatic merging. I've looked into a few different options that use git to sync obsidian between Android and PC, but they all fall short because they don't include automatic merging. It's a pain to have to resolve conflicts all the time.

There are two main files which need to be created at the root of your directory (where `.git` folder resides).
```
touch .gitignore
touch .gitattributes
```
First, you should add these lines to `.gitignore`:
```
.trash/
.obsidian/workspace
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/app.json
```
This tells git to ignore certain files that are likely to cause conflicts, like recently opened files.

Next, you'll add the `.gitattributes` file and tell git how to handle merging for specific file types. For example, you can tell git to treat markdown files as regular text files and merge them accordingly:
```
*.md merge=union
*.json merge=ours
```
What this does is it instructs git to merge markdown files as if they were plain text. This way, the only time you'll get conflicts is when you change the same line in both files, which is pretty unlikely. For json files, we're telling git to use 'ours' config when JSON file is edited, this is specifically for `obsidian-rtl` plugin configuration.

For this part, I used [this tutorial](https://gist.github.com/Makeshift/43c7ecb3f1c28a623ea4386552712114) for the most part: 
To Use Git on Android:
- First, you'll need to download and install the Termux app from the Google Play Store ([Termux – Apps on Google Play](https://play.google.com/store/apps/details?id=com.termux&hl=en_GB&gl=US)). Once you have it installed, open the app and run the following commands 
```
apt update
apt upgrade -y
pkg install openssh -y
pkg install git -y
termux-setup-storage
```
These commands will update your Termux installation, install the necessary packages for Git, and set up your storage so you can access your files.
- Next, you'll need to add your Git configurations. Run the following commands:
```
git config --global credential.helper store
git config --global user.email "<your_email>"
git config --global user.name "<The name you want on your commits>"
```
Replace `<your_email>` and `<The name you want on your commits>` with your own information.

- Now you're ready to clone a repository. Use the following command:
```
git clone <your repository_github_url>
```
For Authentication, you have two ways. Use [Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token), or use [SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh). I use the second as the first one is blocked in my country. 

You should now have the repository folder on your device and you should be able to open your obsidian files inside its app.

But This is not the end. We want to be able to sync obsidian. Right?

The first step is to add a `.profile` file to your Termux home directory. To do this, run the following commands:
```
cd ~
touch .profile
```
Next, you'll need to add some lines of code to your `.profile` file. These lines will create a function called `sync_obsidian` that will take care of syncing your notes. You should add the following code:
```bash
function sync_obsidian
{
cd /storage/emulated/0/shared/obsidian-notes
git add .
git commit -m "Android Commit"
git fetch
git merge --no-edit
git add .
git commit -m "automerge android"
git push
}
alias ob="sync_obsidian"
alias o="sync_obsidian && exit"
```
Make sure to replace `/storage/emulated/0/shared/obsidian-notes` with the location of your obsidian-notes folder.

The `alias ob="sync_obsidian"` creates an alias `ob` for the function `sync_obsidian`, so that you can call the function by just typing `ob` in the terminal. The `alias o="sync_obsidian && exit"` creates an alias `o` for the function `sync_obsidian` and also exits the terminal after syncing. 

> It's important not to use `git pull` in `sync_obsidian` function since it will prompt you for a merge commit message every time there are conflicts, which can be very annoying. [Why is git prompting me for a post-pull merge commit message? - Stack Overflow](https://stackoverflow.com/questions/11744081/why-is-git-prompting-me-for-a-post-pull-merge-commit-message)


