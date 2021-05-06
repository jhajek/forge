---
layout: post
status: publish
published: true
title: 'Enabling Hyper-V and VirtualBox on the same system'
author:
  display_name: Jeremy Hajek
  login: jhajek
  email: hajek@.iit.edu
author_login: jhajek
author_email: hajek@.iit.edu
date: '2019-01-11 20:39:52 -0600'
date_gmt: '2019-01-11 22:09:52 -0600'
categories:
- software
- virtualization
tags: 
- virtualization
- vagrant
- Hyper-V
- hyperv
comments: []
---

In the course of my research and studies, [VirtualBox](https://virtualbox.org "Virtualbox.org link") has been an invaluable tool.  It has allowed me to utilize a laptop and or desktop PC to the fullest.  First allowing me not to have to worry about dual-booting systems, but then allowed me into carrying multiple different versions and configurations of software.  Combined with [Git](https://git-scm.org "git scm website") and [Github](https://github.com/jhajek "My github repos") now I can quickly clone and rebuild any systems I need.  Invaluable indeed.

In retrospect, there is Hyper-V, which is Microsoft's enterprise grade Virtualization Platform which was released initially to compete with VMWare back in an age long ago.  The product is very powerfull and very simple to use.  It has a very Windows feel to it so the learning curve is small.  It now comes available to install on any Windows 10 Professional or Enterprise Edition (not Home).  Why do I mention this?

Well I am currently doing work with the [Microsoft HoloLens](https://microsoft.com/hololens "Microsoft HoloLens website") which is Microsoft's cutting edge Augmented Reality platform.   We have done some neat research with it.

[HoloLens Work at IIT in the Smart Tech Lab](https://forge.sat.iit.edu/augmentedreality/mixedreality/2018/05/25/hololens-work.html
 "HoloLens Work")

But the best part is that the HoloLens makes use of Unity3d and VisualStudio to build and compile appx HoloLens projects.   There is a HoloLens simulator you can use that runs as a Hyper-V virtual machine for testing.  So the need to have both platforms comes into play.   But Hyper-V being a Type II hypervisor and VirtualBox being a Type-I will install together but only one hypervisor can run at a time.  

This is an issue because Hyper-V is loaded at boot and become the default hypervisor in this case.  Then how can you run both at the same time?  This problem goes back pretty far, around 2012 and the release of Windows 8.  [Scott Hanselman had the original trick to solve this problem](http://www.hanselman.com/blog/SwitchEasilyBetweenVirtualBoxAndHyperVWithABCDEditBootEntryInWindows81.aspx "Hanselman solves the problem of Hyperv and virtualbox at the same time").  His solution is to modify the Windows bootloader, called [BCD](https://en.wikipedia.org/wiki/Windows_Vista_startup_process#Boot_Configuration_Data "Windows bootloader"), to make a duplicate boot entry with the flag to disable Hyper-V at boot.  

This can be easily done via launching an elevated **CMD** or **Command Line**, not a Powershell, but a CMD as the syntax of these commands won't be recognized in *powershell*.  Here are the commands given by David Finster at his excellent website, [Finster Tech Consulting](https://fortc.com/switch-between-hyper-v-and-virtualbox-on-windows-10/ "Hyper-V BCD edit solution"):

1. Open a command prompt as administrator.
2. Run the following command: ```bcdedit /copy {current} /d “Disable Hyper-V”```
  * You will see the following result: The entry was successfully copied to (something like) {c8f8754a-d33e-11e8-8234-8cec4b60f7ca}
  * You can use your own description instead of “Disable Hyper-V” if you like.
3. Run the following command: ```bcdedit /set {c8f8754a-d33e-11e8-8234-8cec4b60f7ca} hypervisorlaunchtype off```
  * You will see the following result: The operation completed successfully.
  * Your serial number will be different -- adjust accordingly.
4. Reboot the machine while holding down the shift key. You will be presented with a boot option to “Use another operating system”.
  * Select your new “Disable Hyper-V” option. The machine will boot with Hyper-V disabled and VirtualBox will work.

It works perfectly!
