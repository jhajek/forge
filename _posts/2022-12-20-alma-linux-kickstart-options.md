---
layout: post
status: publish
published: true
title: 'Alma Linux 9 Kickstart Config File New Configuration'
tags: 
- almalinux
- kickstart
- repos
comments: []
---

Seeing how the US National Laboratory, Fermi Lab and CERN have [selected Alma Linux as their default platforms](https://www.theregister.com/2022/12/08/cern_fermilab_almalinux/ "webpage labs select Alam Linux"). I decided to get some Alma experience. Alma is Latin for the word, "home."

[Alma Linux](https://almalinux.org/ "webpage for Alma Linux") is a fork of CentOS, which was an opensource compatible implementation of RHEL Linux (minus the trademarked logos). On December 8, 2020, IBM's Red Hat announced the [discontinuation of CentOS](https://www.theregister.com/2020/12/09/centos_red_hat/ "webpage about discontinue CentOS"). Alma Linux and [Rocky Linux](https://rockylinux.org "webpage for rocky linux") were both born at this time.

## Installing Alma via Kickstart

Using the automated installer for Red Hat based operating systems, called Kickstart, I wanted to install Alma Linux. The first thing I tried was to take a previously working kickstart file, update the installation mirror version, and should be good to go. I have been using CentOS for a long while and had this line that would install the required base OS packages needed for installation.

```
# Centos 8
#url --mirrorlist="http://mirrorlist.centos.org/?release=8&arch=x86_64&repo=BaseOS&infra=$infra"
```

The I took that entire kickstart file and modified the installation URL for use to install Rocky Linux 8.6.

```
# Rocky Linux 8.6
# https://forums.rockylinux.org/t/installation-source-error/3831/4
url --mirrorlist="https://download.rockylinux.org/pub/rocky/8/BaseOS/x86_64/os/"
```

I assumed then since Alma Linux was based off the same codebase I could take my Rocky Linux mirror and just change the values... I was wrong. I received and error relating to kickstart software selection, basically the kickstart couldn't find where any of software needed for install was located.

## Solution

Finding the [Alma Linux Discourse](https://almalinux.discourse.group "webpage for almalinux discourse"), [I found some help](https://almalinux.discourse.group/t/does-almalinux-work-with-kickstart/1538/6 "webpage showing missing parts of kickstart") that would show me what I was missing. Adding this code in place of the previous url line in my kickstart that had worked for Rocky Linux and CentOS, now I have 4 lines to do the job. It works for 9.1 as well, just add the `.1` after the `9`.

```
# set installation source
url --url="https://repo.almalinux.org/almalinux/9/BaseOS/x86_64/kickstart/"
repo --name="alamalinux9-baseos" --baseurl="https://repo.almalinux.org/almalinux/9/BaseOS/x86_64/os/" --mirrorlist=""
repo --name="alamalinux9-appstream" --baseurl="https://repo.almalinux.org/almalinux/9/AppStream/x86_64/os/" --mirrorlist=""
repo --name="epel9-everything" --baseurl="" --mirrorlist="https://mirrors.fedoraproject.org/mirrorlist?repo=epel-9&arch=x86_64"
```

I would like to say thanks to this user who provided the post: [mikew](https://almalinux.discourse.group/u/mikew "webpage profile for user mikew"). There is nothing stating this online and very little clear documentation about using kickstart for Alma Linux.
