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
   openSUSE_Tumbleweed&nbsp;&nbsp;&nbsp; x86_64  
   openSUSE_Tumbleweed&nbsp;&nbsp;&nbsp; i586  
   openSUSE_Leap_42.3&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; x86_64  
   openSUSE_Leap_15.0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; x86_64  
       SLE_12_SP1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; x86_64  
   SLE_12_Backports&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; s390x  
   {:.highlight}
   
   The output is a two column table. First column represents the REPOSITORY and second column represents ARCH. Lets say you are intersted in s390x and SLE_12_Backports then use the following command to build the package
   
   $ osc build SLE_12_Backports  s390x
   {:.highlight}

6. **Step: Test the Package**
   
   If the package is successfully built that means, there are no build errors and you can further test your changes by installing it on the machine. If you are using zypper you can install the package
   
   $ zypper in -f `<PATH-TO-RPM`>
   {:.highlight}
   
   Once you are satisfied that the changes you have made are working fine you are ready to commit the changes to server.
7. **Step: Record Changes**
   
   The _changes_ files should be updated to record changes you made to the package. If you have fixed a bug you can refer to it using bnc#xxxxxx. Run following command in local directory to edit the changes file
   
   $ osc vc
   {:.highlight}
8. **Step: Update Version Control Status**
   
   Now its time to update the version control status. Run following command in local directory
   
   $ osc addremove
   {:.highlight}
   
   After doing this make sure there are no files with status “!” or “?”. You can check this by running the command
   
   $ osc status
   {:.highlight}
   
9. **Step: Commit to OBS Server**
   
   If all the files have correct status you are ready to upload the local source files into the already branched package on OBS server. Once upload completes an automatic re-build is triggered for the package. Run the following command to commit
   
   $ osc commit
   {:.highlight}
   
10. **Step: Verify Build Result on Server**

    Once the build at server is completed you should verify that build is "succeeded" for all build repositories that are enabled for the original package. You can check this through webpage or by using following osc command in local directory
   
   $ osc results –verbose
   SLE_12_SP3&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;s390x&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;succeeded(unpublished)
SLE_12_SP3&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;arch64&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;failed
SLE_12_SP3&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ppc64le&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;succeeded(unpublished)
SLE_12_SP2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; x86_64&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;succeeded(unpublished)
SLE_12_SP1_Backports&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;x86_64&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;succeeded(unpublished)
SLE_11_SP4&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i586&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;disabled
{:.highlight} 
   
11. **Step: Submit Changes to Original Package**
   
    If build for all the desired repositories have a "succeeded" status create a submit request to original package by running the following command
   
   $ osc submitrequest --message='Right a short message that tells about your changes' home:testuser:branches:server:monitoring cacti server:monitoring cacti
   {:.highlight}
   
   This will create a request from your branched package to original package. The command will ouput a request id remember that. The maintainer will either accept or decline the request.
