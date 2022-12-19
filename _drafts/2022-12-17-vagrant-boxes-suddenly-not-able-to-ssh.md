---
layout: post
status: publish
published: true
title: 'Vagrant Boxes Suddenly Not Able to SSH into'
tags: 
- vagrant
- packer
- ssh
comments: []
---

Being a user of [Hashicorp Packer](https://packer.io "website for hashicorp packer") and [Vagrant](https://vagrantup.com "website for vagrant") I was familiar with their ins and outs. In fact adopting these products back in 2011 and 2012 I have seen the wave of features and had built up quite a custom library of my own virtual machine builds.

## Background and Software Versions

Recently I began to experience a unique problem.  I was using Vagrant version 2.3.2 then 2.3.4, Packer 1.7.5 and then 1.8.4 and VirtualBox 1.6.38 then 1.6.40. Installed via Chocolatey.

## Odd Error

I was receiving an odd error. I use many of the stock Ubuntu Vagrant boxes, Jammy and Focal as well as build my own Virtual Machines via Packer. On Windows 10 before the December 2022 patch Tuesday and after. The odd error I encountered was this everytime I issued `vagrant ssh`:

`vagrant@127.0.0.1: Permission denied (publickey)`

The worst part of this is that it was happening on the Virtual Machines that was the result of a Packer build template. I checked and rechecked all my templates and I couldn't find a problem.  All Virtual Machines, would correctly generate a new SSH RSA keypair after the initial insecure Vagrant key-pair was removed.

I checked by launching some of my existing Vagrant Boxes and some of the stock boxes from Vagrantup.com and they worked. Newly built virtual machines and newly `vagrant destroyed` boxes were then getting the permission denied publickey error.

After checking to make sure all the proper keys are inserted I wanted to see some of the verbose SSH output. I issued the command: `vagrant ssh -- -v`, which passes the verbose flag to print out the ssh connection negotiation and what did I find?

## Permission is too Open on Windows

I found this:

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions for '.vagrant/machines/default/virtualbox/private_key' are too open.
```

Hmm so it seems that Vagrant on Windows when generating the new RSA key pair for each new Vagrant Box, was generating a key pair with too open permissions, which SSH will by default not proceed with and throw this error.  

In this case the `vagrant ssh` abstraction was hiding this from me. I needed to add the addition `-- -v` to see the verbose output.

## Solution

So on Windows you need to change the permission of the private key file.  If you navigate to the path, say `.\focal64\.vagrant\machines\default\virtualbox` in the file manager you will see the `private_key` file.  If you `right-click` and select `properties > security` you will see that the file has no owner.

You should select your own user, by hitting the `Edit` button and adding your username, then give your user full control (as you are the user who will need to use the key).

This can also be done via the `icacls` command line (not from PowerShell but good old fashion CMD). The good folks at [StackOverFlow have an answer as well](https://stackoverflow.com/questions/48888365/openssh-using-private-key-on-windows-unprotected-private-key-file-error "website for stack overflow answer")

```
# From the Command Prompt (CMD) not PowerShell
icacls .\private.key /inheritance:r
icacls .\private.key /grant:r "%username%":"(R)"
```

## But Wait There is More

Two more Items: my packer built Vagrant Boxes have the opposite problem, they have multiple user accounts owning the private key...

The focal64 box's permission I just fixed manually, but when I issue the command: `vagrant destroy -f ; vagrant up ; vagrant ssh` I am right back to the permission denied error on the generation of a new RSA Key pair.

Even worse, systems where `vagrant ssh` work, after a `vagrant destroy` the permissions are all messed up and the `permission denied` error comes back.
