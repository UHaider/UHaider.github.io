---
layout: post
title: Contributing to SUSE Package Hub Step-by-Step
---
## Introduction

If you are reading this post that means you are interested in contributing to [Package Hub](https://packagehub.suse.com). You have come to the right place. I’ll share with you the process I followed while doing my internship at [Linux Foundation](https://www.linuxfoundation.org/). During the internship I worked on different softwares and packaged them for **_s390x_** machines. Thanks to [IBM LinuxONE Community Cloud](https://developer.ibm.com/linuxone/) because of which I was able to get a s390x machine for packaging and testing of ported packages. If you are interested you can request a three month [trial](https://linuxone20.cloud.marist.edu/cloud/#/register). I hope this blog post will help you understand the process better. Please remember to visit SUSE documentation also to get more insight and latest updates.
{:.justify-class}

### SUSE Package Hub
[SUSE Package Hub](https://packagehub.suse.com/) contains popular open source packages for [SUSE Linux Enterprise Server (SLES)](https://www.suse.com/products/server/). SUSE Package Hub packages are built and maintained by a community of users and "packagers" utilizing the [Open Build Service (OBS)](https://build.opensuse.org/). That means you don't need to build everything alone yourself! If you want to use an open source software package on your SLES and want that SLES remains supported and supportable when using the software, you should use the package available in SUSE Package Hub. If you need a newer version or the software package is not available in SUSE Package Hub you can always contribute and the purpose of this post is to explain the process of contributing to SUSE Package HUB using Open Build service.
{:.justify-class}

### openSUSE Backports
[openSUSE Backports or openSUSE:Backports](https://en.opensuse.org/Portal:Backports) is the community project maintaining the packages that feed into SUSE Package Hub. These packages are built using the openSUSE Build Service a publically hosted instance of the Open Build Service. The packages are essentially copies of packages from openSUSE distributions built for use with SUSE Linux Enterprise products.
{:.justify-class}

### Factory Project
Factory is built in its own project [openSUSE:Factory](https://build.opensuse.org/project/show/openSUSE:Factory) on public OBS server. The project contains large number of packages and maintains the rolling development codebase for **_openSUSE Tumbleweed_** and **_openSUSE Leap_** distributions. The exact same packages qualified and approved for those distributions is used for the SUSE Package Hub. So, any package that is going into SUSE Package Hub must be first checked into [openSUSE:Factory](https://build.opensuse.org/project/show/openSUSE:Factory).
{:.justify-class}

### Devel Projects
All the development work for the packages in openSUSE:Factory project is done in packages present in respective **_devel projects_**. All the contributions to a package like bug fixing, version upgrate etc, are submitted to the pacakge in _devel project_. You can get a list of current devel projects that are feeding to openSUSE:Factory [here](https://build.opensuse.org/stage/project/status?project=openSUSE%3AFactory).
{:.justify-class}


### Concept
The main concept is that in order to contribute to SUSE Package Hub you need to have an account on OBS public server. Then you can submit your work to a devel project and subsequently to openSUSE:Factory. Once your contribution reaches in openSUSE:Factory project you are ready to create a submit (maintenance) request to openSUSE:Backports. If your request to Backports is accepted your work will be automatically submitted to SUSE Package Hub.
{:.justify-class}

### Getting Ready
SUSE Package Hub packages are built and maintained utilizing the [Open Build Service (OBS)](https://build.opensuse.org/). OBS system enables developers and package maintainers to build and distribute packages from sources in an automatic, consistent and reproducible way. So, first you need to **register an account** at OBS server [here](https://build.opensuse.org/). After registering the account, you should install and setup the [OSC](https://en.opensuse.org/openSUSE:OSC) tool on your computer. The rest of the blog assumes that you have completed these two steps.
{:.justify-class}

## Contributing to Existing Software Package:

Open build service contains almost all popular open source software packages and chances are that the software you are interested in is already present in a project on OBS server. Let us assume that the package you are interested in is **_already present in openSUSE:Factory_** project and you want to contribute to that. The contribution could be a bux fix, version upgrade, adding a functionality or even adding support for another architecture etc. As said earlier, when you are satisfied with the package in openSUSE:Factory you can send the package to Package Hub by a making a submit request. Let us assume that the original software package you want to work on is **_cacti_** and OBS username is **_testuser_**
{:.justify-class}

> `<username>` testuser <br/>
> `<original_package>` cacti


Now, let us now look at the steps invloved  
1. **Step: Devel Project**

   Find the _devel_ project for the package and branch from there. To find _devel_ project you can use [OSC](https://en.opensuse.org/openSUSE:OSC). For example, if you want to know about the _devel_ project of the cacti package in _openSUSE:Factory_ project you can use following command
   {:.justify-class}
   
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
   {:.justify-class}
   
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
   {:.justify-class}
   
   $ osc repos  
   openSUSE_Tumbleweed&nbsp;&nbsp;&nbsp; x86_64  
   openSUSE_Tumbleweed&nbsp;&nbsp;&nbsp; i586  
   openSUSE_Leap_42.3&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; x86_64  
   openSUSE_Leap_15.0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; x86_64  
       SLE_12_SP1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; x86_64  
   SLE_12_Backports&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; s390x  
   {:.highlight}
   
   The output is a two column table. First column represents the REPOSITORY and second column represents ARCH. Lets say you are intersted in s390x and SLE_12_Backports then use the following command to build the package
   {:.justify-class}
   
   $ osc build SLE_12_Backports  s390x
   {:.highlight}

6. **Step: Test the Package**
   
   If the package is successfully built that means, there are no build errors and you can further test your changes by installing it on the machine. If you are using zypper you can install the package
   {:.justify-class}
   
   $ zypper in -f `<PATH-TO-RPM`>
   {:.highlight}
   
   Once you are satisfied that the changes you have made are working fine you are ready to commit the changes to server.
7. **Step: Record Changes**
   
   The _changes_ files should be updated to record changes you made to the package. If you have fixed a bug you can refer to it using bnc#xxxxxx. Run following command in local directory to edit the changes file
   {:.justify-class}
   
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
   {:.justify-class}
   
   $ osc commit
   {:.highlight}
   
10. **Step: Verify Build Result on Server**

    Once the build at server is completed you should verify that build is "succeeded" for all build repositories that are enabled for the original package. You can check this through webpage or by using following osc command in local directory
    {:.justify-class}
   
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
    {:.justify-class}
   
    $ osc submitrequest --message='Right a short message that tells about your changes' home:testuser:branches:server:monitoring cacti server:monitoring cacti
    {:.highlight}
   
    This will create a request from your branched package to original package. The command will ouput a request id remember that. The maintainer will either accept or decline the request.
    {:.justify-class}

12. **Step: Status of Request**
   
    You can check the status of your submit request using osc. You can get the username of the maintainer and his email address using osc commands. This could be useful if there is a need to contact the maintainer
    {:.justify-class}
    
    $ osc request show `<submit_request id>`  
    _Request hisotry will be printed_  
    $ osc maintainer server:monitoring cacti  
    `<maintainer_username`>  
    $ osc  whois `<maintainer_username>`  
    _Contact information will be printed_  
    {:.highlight}
 
13. **Step: Submit to Facatory**
   
    If the maintainer is happy with your submit request and accepts it, you are now ready to submit the changes to Factory. Make a submit request to Factory
    {:.justify-class}
       
       $ osc submitrequest –message='A messge for Factory' server:monitoring/cacti openSUSE:Factory cacti
       {:.highlight}
14. **Step: Submit to Backports**
   
    After request to Factory is accepted. Make a submit request, a maintenance request, to Backports (SLE12) using the following command 
    {:.justify-class}
   
    $ osc submitrequest openSUSE:Factory/cacti openSUSE:Backports:SLE-12
    {:.highlight}
    
    Once the Package Hub team is happy with the request the package will be moved to SUSE Package Hub.


## Contributing New Software Package:

A new software package, package not already in Factory, can be submitted to openSUSE factory through a **_devel_** project. Devel projects act as feeders to openSUSE:Factory project. Packages from home: namespace are not allowed to be directly submitted to Factory. You can get a list of current devel projects that are feeding to openSUSE:Factory [here](https://build.opensuse.org/stage/project/status?project=openSUSE%3AFactory).
{:.justify-class}

1. **Step: Select Devel Project**
   
    So, first you need to find an appropriate devel project where you can maintain your package. For example if you want to maintain a package that provides network monitoring functionalities an appropriate devel project would be server:monitoring. If you think you need a new devel project you can always ask the maintainers of an existing top-level project and ask them to create a subproject for you. If there is no suitable top-level project then you can ask OBS maintainers to create a project for you by opening a [bug]( https://bugzilla.opensuse.org/enter_bug.cgi?product=openSUSE.org&component=BuildService&short_desc=New%20Project%20Space
) or contacting them at <admin@opensuse.org>.
{:.justify-class}

    So you have a new devel project for your new package but Factory does not know about them yet. The submit request from this new devel project will be auto-declined by Factory project. So, the new project needs to be added to devel-whitelist and you can do this by asking in #openSUSE-factory IRC channel. The new project will be able to successfully feed the packages to Factory once it added to devel-whitelist.
{:.justify-class}

2. **Step: Submit Package to Devel project**
   
    After you have a devel project, which is able to feed to Factory, you can now submit your  package to this devel project.
    {:.justify-class}
   
    $ osc submitrequest -m ' Maintaining `<package-name`> in Factory with `<devel-project-name`> as devel project.'  home:username/`<package-name`>  `<devel-project-name`>
    {:.highlight}
 
 3. **Step: Submit Package to Factory**
   
    Submiting to Factory is easy now, just create a submit request from your devel project to Factory. Please write a description of your new package in the submit request so that it can serve as an introduction to openSUSE:Factory.
    {:.justify-class}
   
    $ osc submitrequest -m ' This is a new package. Add description or a link that gives detail about the new package. ' `<devel-project-name`> openSUSE:Factory
    {:.highlight}

 4. **Step: Submit Package to Backports**
   
    Submit to Backports to get the package in to SUSE Package Hub (SLE12)
    {:.justify-class}
   
    $ osc submitrequest openSUSE:Factory/`<package-name`>  openSUSE:Backports:SLE-12
    {:.highlight}
    
    Again once the Package Hub team is happy with the request the package will be moved to SUSE Package Hub.
    {:.justify-class}
    
## Conclusion
In this post, I tried to summarize the steps involved in contributing to openSUSE:Factory project and SUSE Package Hub. I hope that after reading this blog post you have gained more understanding on how to contribute to Factory and SUSE Package Hub using Open Build Service.
{:.justify-class}
## Acknowledgements

I thank [Linux Foundation](https://www.linuxfoundation.org/) for providing me with the opportunity to work on this great open source project. I also thank my mentor Wolfgang Engel from SUSE who guided and helped me in uderstanding the process. 
