---
layout: post
title: Contributing to Package Hub Step-by-Step
---

After reading this blog post you will be able to make contributions to Package Hub using Open Build Service.
{:.justify-class}

If you are reading this blog that means you are interested in contributing to [Package Hub](https://packagehub.suse.com). You have come to the right place. I’ll share with you the process I followed while doing my internship at [Linux Foundation](https://www.linuxfoundation.org/). I hope this blog post will help you understand the process better. Please remember to visit suse documentation also to get more insight and latest updates.
{:.justify-class}

SUSE Package Hub packages are built and maintained utilizing the [Open Build Service (OBS)](https://build.opensuse.org/). OBS system enables developers and package maintainers to build and distribute packages from sources in an automatic, consistent and reproducible way. So, first you need to register an account at OBS. After registering the account, you should install and setup the osc tool on your computer. The rest of the blog assumes that you have completed these two steps.
{:.justify-class}

## Contributing to Existing Software Package:

Open build service contains almost all popular open source software packages and chances are that the software you are interested in is already present in a project on OBS server. If you are interested in an open source software and you want that software to be present in package hub then search if the package is already present on OBS server or not. If yes, then you have less work to do. Let us assume that the original software package you want to work on is cacti and OBS username is testuser
{:.justify-class}

> `<username>` testuser <br/>
> `<original_package>` cacti


Let us now look at the steps 
1. **Step 1: Devel Project**

   Find the _devel_ project for the package and branch from there. To find _devel_ project you can use [OSC](https://en.opensuse.org/openSUSE:OSC). For example, if you want to know about the _devel_ project of the cacti package in _openSUSE:Factory_ project you can use following command
   
   $ osc develproject openSUSE:Factory cacti <br/>
   server:monitoring
   {:.highlight}
   So server:monitoring is the _devel_ project for openSUSE:Factory/cacti package.
2. **Step 2: Branching**
   
   Now branch the pacakge from the _devel_ project using the command
   
   $ osc branch server:monitoring cacti
   {:.highlight}
   This will create a new branch project 
   > home:`<your_user_name>`:branches:`<original_project_name>` 

2. **Step 3: Checkout**
   
   You should now checkout the package to download all the files from the server to a local directory. The generic command for checking out a branched package is 
   
   $ osc checkout home:<your_user_name>:branches:<original_project_name>/<original_package_name>
   {:.highlight}
   
   In our case the command will be
   
   $ osc checkout home:testuser:branches:server:monitoring/cacti
   {:.hightlight}
   
   This will download the files from the server to local directory
   > home:testuser:branches:server:monitoring/cacti

