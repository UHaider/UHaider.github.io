---
layout: post
title: Contributing to Package Hub Step-by-Step
---

After reading this blog post you will be able to make contributions to Package Hub using Open Build Service.
{:.justify-class}

If you are reading this blog that means you are interested in contributing to [Package Hub](https://packagehub.suse.com). You have come to the right place. I’ll share with you the process I followed while doing my internship at [Linux Foundation](https://www.linuxfoundation.org/). I hope this blog post will help you understand the process better. Please remember to visit suse documentation too to get more insight and latest updates.
{:.justify-class}

SUSE Package Hub packages are built and maintained utilizing the [Open Build Service (OBS)](https://build.opensuse.org/). OBS system enables developers and package maintainers to build and distribute packages from sources in an automatic, consistent and reproducible way. So, first you need to register an account at OBS. After registering the account, you should install and setup the osc tool on your computer. The rest of the blog assumes that you have completed these two steps.
{:.justify-class}

## Contributing to Existing Software Package:

Open build service contains almost all popular open source software packages and chances are that the software you are interested in is already present in a project on OBS server. If you are interested in an open source software and you want that software to be present in package hub then search if the package is already present on OBS server or not. If yes, then you have less work to do. Let us assume the following  {:.justify-class}

> <userrname> testuser
> <orignal_package> cacti

