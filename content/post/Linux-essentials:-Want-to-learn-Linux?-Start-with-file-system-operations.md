---
title: "Linux Essentials: Want to Learn Linux? Start With File System Operations"
date: 2022-02-10T15:28:48+03:00
lastmod: 2022-02-10T15:28:48+03:00
draft: false
tags: ["linux"]
categories: ["Linux"]
author: "Vincent N."

<p align="center">
    <img src="../img/linux-screen.mp4" width="650" height="400"/>

</p>

## **Introduction**
Put the terms 'free', 'open source' and 'software' in one sentence, talk of tech-gasm! Linux has attracted a lot of users with genuine love for it because of this nature thanks to it's founder Linus Torvalds and it's open source community that continue to support and add developments.  
Linux is a family of open-source Unix-like operating systems based on the Linux kernel. It is typically packaged in a Linux distribution. Popular Linux distributions include Debian, Fedora Linux, and Ubuntu.  
In this article I share with you how to get started learning linux, kicking your journey off with basic file system operations like traversing through directories, listing files etc using the terminal.

## **Linux terminal**

<p align="center">
    <img src="../img/linux-terminal.png" width="650" height="400"/>
    
</p>

A terminal is a command line interface (CLI) where you run your commands to perform specific tasks. The kernel acts as the middle man between you and the operating system, when you issue the command, it gets interpreted to the OS in appropriate format by the kernel where the OS then performs the intended task as directed by you.  
In Linux, you can either run commands as the root user (admin) or as a normal user. The root user has all the privileges to execute any command while a normal user has limited access to some operations. It is however advisable to run commands as a normal user even when you are the administrator because things can and do go wrong sometimes.

## **Commands for basic file system operations**
We'll go over basic linux commands to help you get started with traversing through directories and performing basic file system operations. These include:
- printing the current working directory
- listing files in a directory
- moving into a directory
- creating directories
- creating empty files
- appending to and over-writing files
- displaying file contents
- copying files
- moving files to another directory
- renaming files
- deleting files
- deleting directories

Once you fire up a terminal, type the following command to print current working directory:


```shell
pwd
```

    /home/lyrax/Projects/blog/Downloads


This is the path to the directory we are currently in.  
To list files in the directory, we use the _ls_ command:

<p align="center">
    <img src="/img/blog/linux-fso/linux-ls.png" width="500" height="300"/>
    
</p>

Linux commands have parameter options associated with them, you can use these to dictate how you want a command to function. For example there may be hidden files in the directory and we want to print every file. In that case we pass the _-a_ option to _ls_ as follows:

<p align="center">
    <img src="/img/blog/linux-fso/linux-lsa.png" width="500" height="300"/>
    
</p>

We see that the three hidden files have now been output.  
Let's move into the _'bets ml'_ directory. In linux we say changing directories and we use _cd_ command (change directory).

<p align="center">
    <img src="/img/blog/linux-fso/linux-cd.png" width="500" height="300"/>
    
</p>

To ascend back to our previous directory, we use two dots and do _cd_ this way:
<p align="center">
    <img src="/img/blog/linux-fso/linux-cd-dots.png" width="500" height="300"/>
    
</p>

Creating a directory:  
We use _mkdir_ command (make directory) followed by the name we want to call give the new folder.  
Let's call our new directory _'movies'_ :

<p align="center">
    <img src="/img/blog/linux-fso/linux-mkdir.png" width="500" height="300"/>
    
</p>

We have created a new directory _'movies'_  
Let's create an empty text file. This comes in handy when you want to create a file fast without having to open a file reader application.  
We use the command _'touch'_ followed by the name we want to give our file:

<p align="center">
    <img src="/img/blog/linux-fso/linux-touch.png" width="500" height="300"/>
    
</p>

We have created an empty text file that we have called 'army'.  

Let's write to the empty file. We want to add a line 'this is a text file' into it. We use the _'echo'_ command to write and the _'cat'_ command to view file contents as follows:

<p align="center">
    <img src="/img/blog/linux-fso/linux-echo-cat.png" width="500" height="300"/>
    
</p>

To add another line without over-writing the file contents (known as to append), we use double '>>' signs. Using a single '>' rewrites the file contents thus overwriting it. Study how the file contents change when we do the following:  

<p align="center">
    <img src="/img/blog/linux-fso/linux-echo-cat2.png" width="500" height="300"/>
    
</p>


How about copying files? We use the _'cp'_ command along with the file path and destination path to copy to as follows:

<p align="center">
    <img src="/img/blog/linux-fso/linux-cp.png" width="500" height="300"/>
    
</p>

We have copied the text file to the 'bets ml' directory.  
We may also want to move a file instead of copying it. We use the _'mv'_ command:

<p align="center">
    <img src="/img/blog/linux-fso/linux-mv.png" width="500" height="300"/>
    
</p>

Now our text file is no longer in the movies directory but in the 'bets ml' folder.  
We can also specify the name we want to give our moved file in the new directory. Say for example we want to call our text file navy.txt once it is moved, we specify the absolute path this way:

<p align="center">
    <img src="/img/blog/linux-fso/linux-mv2.png" width="500" height="300"/>
    
</p>

That is also how we rename files in the same directory, using the _mv_ command.

To delete files in a directory, we use the _'rm'_ command (remove).

<p align="center">
    <img src="/img/blog/linux-fso/linux-rm.png" width="500" height="300"/>
    
</p>

We have deleted the text file.  
To delete directories, we use _'rmdir'_ (remove directory) for empty directories or _'rm -r'_. We specify the option _'-r'_ to mean recursively, to remove non-empty directories.  

<p align="center">
    <img src="/img/blog/linux-fso/linux-rmdir.png" width="500" height="300"/>
    
</p>


## **Getting help on the terminal**
Linux comes with pre-built-in documentation for its commands. You can access the command help information using the _'man'_ command.  
Say for example we want to learn more about the _'wc'_ command. We can run _'man wc'_ or _'wc --help'_ from the command line to view the documentation or help information for this command:

<p align="center">
    <img src="/img/blog/linux-fso/linux-man-help.png" width="700" height="400"/>
    
</p>

The output on the right is the detailed documentation for _'wc'_ command.

Man is the command to turn to when you're stuck or when you want to learn more about a command, Man!