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
1. **Step: Devel Project**

   Find the _devel_ project for the package and branch from there. To find _devel_ project you can use [OSC](https://en.opensuse.org/openSUSE:OSC). For example, if you want to know about the _devel_ project of the cacti package in _openSUSE:Factory_ project you can use following command
   
   $ osc develproject openSUSE:Factory cacti <br/>
   server:monitoring
   {:.highlight}
   So server:monitoring is the _devel_ project for openSUSE:Factory/cacti package.
2. **Step: Branching**
   
   Now branch the pacakge from the _devel_ project using the command
   
   $ osc branch server:monitoring cacti
   {:.highlight}
   This will create a new branch project 
   > home:`<your_user_name>`:branches:`<original_project_name>` 

3. **Step: Checkout**
   
   You should now checkout the package to download all the files from the server to a local directory. The generic command for checking out a branched package is 
   
   > osc checkout home:`<your_user_name`>:branches:`<original_project_name`>/`<original_package_name`>
   
   
   In our case the command will be
   
   $ osc checkout home:testuser:branches:server:monitoring/cacti
   {:.highlight}
   
   This will download the files from the server to local directory
   > home:testuser:branches:server:monitoring/cacti
   
   Go to the directory and set usual default mask to 0022
   $ cd home:testuser:branches:server:monitoring/cacti
   $ umask 0022

4. **Step: Make Changes**
   
   Go to the local directory and make changes you want. You may
     * Fix a bug in the package
     * Make changes to specfile (eg to add support for an architecture like s390x, or to update the version of package)

5. **Step: Building Package**
   Locally build the package to verify any changes you made. To get the possible build targets for your package use following command inside the local directory
   
   $ osc repos
   openSUSE_Tumbleweed                x86_64 <br/>
   openSUSE_Tumbleweed                i586   <br/>
   openSUSE_Leap_42.3                 x86_64 <br/>
   openSUSE_Leap_15.0                 x86_64 <br/>
   SLE_12_SP1                         x86_64 <br/>
   SLE_12_Backports                   s390x  <br/>
   SLE_12                             x86_64 <br/>
   {:.highlight}
   
   The output is a two column table. First column represents the REPOSITORY and second column represents ARCH. Lets say you are intersted in s390x and SLE_12_Backports then use the following command to build the package
   
   $ osc build SLE_12_Backports  s390x
   {:.highlight}
   
   
