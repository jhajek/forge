---
layout: post
status: publish
published: true
title: Bundling an ubuntu Desktop custom image with VNC access
author:
  display_name: ktewoldu
  login: ktewoldu
  email: ktewoldu@iit.edu
  url: ''
author_login: ktewoldu
author_email: ktewoldu@iit.edu
wordpress_id: 55
wordpress_url: http://aurora.sat.iit.edu/?p=55
date: '2011-08-03 15:17:46 -0500'
date_gmt: '2011-08-03 20:17:46 -0500'
categories:
- tools
- Projects
- Cloud
tags:
- Eucalyptus
- HP
comments: []
---
### Bundling an Ubuntu Desktop custom image with VNC access

Before beginning, make sure the following packages are installed on the Ubuntu Desktop machine that you will be using to create the custom image:

* qemu-kvm - sudo apt-get install qemu-kvm
* kvm-px - sudo apt-get install kvm-px
* xtightvncviewer - sudo apt-get install xtightvncviewer
* curl -sudo apt-get install curl
* openssh-server - sudo apt-get install openssh-server

Here it is as a one liner
<pre lang="bash">sudo apt-get install qemu-kvm kvm-px xtightvncviewer curl openssh-server links</pre>
<p style="text-align: left;" align="center">Also, make sure the .iso of the Ubuntu Desktop image to be bundled exists in the home directory (i.e. /home/user01) of the user that you will be logged in as while creating the custom image.
**<span style="text-decoration: underline;">Creating the disk image:</span>**
The following command creates the image of the virtual hard drive that KVM emulates. In this example, we are creating a virtual hard drive 5 gigabytes in size named image.img. You can change the name and size of the image to whatever you&Gamma;&Ccedil;&Ouml;d like, just make sure to use the same name in all of the commands that follow:
<pre lang="bash">kvm-img create -f raw image.img 5G</pre>
<span style="text-decoration: underline;">Installing the VM:</span>
To start the installation process, execute the following command from the home directory (this is a single command):
<pre lang="bash">sudo kvm -m 512 -cdrom NAME_OF_IMAGE_HERE.iso &Gamma;&Ccedil;&ocirc;drive file=image.img,if=scsi,index=0 -boot d -net nic -net user -nographic -vnc :1</pre>
If your installation process requires more than 512MB of RAM change the -m option, and if you need more processors available, you can use the -c option. After executing the above command, the prompt will disappear from the terminal. Open another terminal window and execute the following command to access the VM:
<pre lang="bash" line="1">vncviewer 0.0.0.0:1</pre>
After finishing the installation, relaunch the VM by executing the following command (this is a single command):
<pre lang="bash" line="1">sudo kvm -m 512 &Gamma;&Ccedil;&ocirc;drive file=image.img, if=scsi,index=0, boot=on -boot c -net nic -net user -nographic -vnc :1</pre>
Open another terminal window and execute the following command to access the VM:
<pre lang="bash" line="1">vncviewer 0.0.0.0:1</pre>
At this point you can add all the packages and programs you want to have installed, update the installation, add users and any settings that need to be present in your UEC instances.
Next, install and configure VNC server in the VM:
<pre lang="bash" line="1">sudo apt-get install tightvncserver
vncserver</pre>
Enter and confirm the password.
Edit the file ~/.vnc/xstatup and add the following line:
<code>gnome-session &amp;</code>
Create the file /etc/init.d/vncserver. Now edit the file and add the following lines:
**COPY/PASTE the following line * make sure to put in the user name in export USER= &Gamma;&Ccedil;&pound;&Gamma;&Ccedil;&yen; field**
<pre lang="bash">#!/bin/sh -e
### BEGIN INIT INFO
# Provides:          vncserver
# Required-Start:    networking
# Required-Stop:     networking
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
### END INIT INFO
PATH="$PATH:/usr/X11R6/bin/"
# The Username:Group that will run VNC
export USER="ubuntu"  #in our example we named the user ubuntu
#${RUNAS}
# The display that VNC will use
DISPLAY="1"
# Color depth (between 8 and 32)
DEPTH="16"
# The Desktop geometry to use.
#GEOMETRY="x"
#GEOMETRY="800x600"
GEOMETRY="1024x768"
#GEOMETRY="1280x1024"
# The name that the VNC Desktop will have.
NAME="my-vnc-server"
OPTIONS="-name ${NAME} -depth ${DEPTH} -geometry ${GEOMETRY} :${DISPLAY}"
. /lib/lsb/init-functions
case "$1" in
start)
 log_action_begin_msg "Starting vncserver for user '${USER}' on localhost: ${DISPLAY}"
 su ${USER} -c "/usr/bin/vncserver ${OPTIONS}"
 ;;
stop)
 log_action_begin_msg "Stoping vncserver for user '${USER}' on localhost:${DISPLAY}"
 su ${USER} -c "/usr/bin/vncserver -kill :${DISPLAY}"
 ;;
restart)
 $0 stop
 $0 start
 ;;
esac
exit 0</pre>
Run the following command to make the file executable:
<pre lang="bash">sudo chmod +x /etc/init.d/vncserver</pre>
Run the following commands:
<pre lang="bash">sudo update-rc.d.d vncserver defaults</pre>
<pre lang="bash">sudo update-rc.d vncserver enable</pre>
Now remove the network persistent rules from /etc/udev/rules.d so that the instance always comes up with eth0 as the network interface:
<pre lang="bash">sudo rm -rf /etc/udev/rules.d/70-persistent-net.rules</pre>
<h3>Integrating with UEC</h3>
The new image needs to know what IP it has when started in UEC and also, it needs to have the public key of the user allowed to do a passwordless access through SSH. The way its done in UEC is via a restful interface provided by the cloud. The interface is available under this URL: http://169.254.169.254/latest/meta-data.
First install curl on the VM.
<pre lang="bash">sudo apt-get install curl</pre>
And add the following lines to /etc/rc.local of the image.
<pre lang="bash">depmod -a
modprobe acpiphp
# simple attempt to get the user ssh key using the meta-data service
# assuming user is the username of an account that has been created
mkdir -p /home/user/.ssh
echo >> /home/user/.ssh/authorized_keys
curl -m 10 -s http://169.254.169.254/latest/meta-data/public-keys/0/openssh-key | grep 'ssh-rsa' >>  /home/user/.ssh/authorized_keys
echo "AUTHORIZED_KEYS:"
echo "************************"
cat /home/user/.ssh/authorized_keys
echo "************************"</pre>
Add the above lines before the exit 0 in /etc/rc.local.

#### Transferring the image, kernel, and ramdisk to the Cloud Controller:
Copy the kernel and the initrd image from the VM image to some place outside.
These will be used later for creating and uploading a complete virtual image to eucalyptus. In this guide we will use the cloud controller to to upload the image.

The files that need to be transferred to the Cloud Controller are:

* image.img
* vmlinuz-x.x.xx-xx-xxxxxx
* initrd.img-x.x.xx-xx-xxxxxx

Where represents some number or letter.
The following are the 3 commands to be executed to upload the files to the cloud controller.
**NOTE: Don"t forget the : at the end of each of these commands!:**
<pre lang="bash">sudo scp /boot/image.img user01@<IIP_ADDRESS_OF_CLOUD_CTRLR>:
sudo scp /boot/vmlinuz-x.x.xx-xx-xxxxxx user01@<IP_ADDRESS_OF_CLOUD_CTRLR>:
sudo scp /boot/initrd.img-x.x.xx-xx-xxxxxx user01@<IP_ADDRESS_OF_CLOUD_CTRLR>:</pre>
<h4><span style="text-decoration: underline;">Registering the kernel image:</span></h4>
On the Cloud Controller, execute the 3 following commands to bundle and register the kernel image (the x&Gamma;&Ccedil;&Ouml;s represent numbers and letters):
<pre lang="bash">sudo euca-bundle-image -i vmlinuz-x.x.xx-xx-xxxxxx --kernel true
sudo euca-upload-bundle -b mybucket -m /tmp/vmlinuz-x.x.xx-xx-xxxxxx.manifest.xml
sudo euca-register mybucket/vmlinuz-x.x.xx-xx-server.manifest.xml</pre>
Save the output produced by the last command above (eki-XXXXXXXX), which will be needed while registering the disk image.
<h4><span style="text-decoration: underline;">Registering the ramdisk image:</span></h4>
On the Cloud Controller, execute the 3 following commands to bundle and register the ramdisk image (the x&Gamma;&Ccedil;&Ouml;s represent numbers and letters):
<pre lang="bash">sudo euca-bundle-image -i initrd.img-x.x.xx-xx-xxxxxx
sudo euca-upload-bundle -b mybucket -m /tmp/initrd.img-x.x.xx-xx-server.manifest.xml
sudo euca-register mybucket/initrd.img-x.x.xx-xx-server.manifest.xml</pre>
Save the output produced by the last command above (eri-XXXXXXXX), which will be needed while registering the disk image.
<h4><span style="text-decoration: underline;">Registering the disk image:</span></h4>
On the Cloud Controller, execute the 3 following commands to bundle and register the ramdisk image. <span style="text-decoration: underline;">Replace eki-XXXXXXXX and eri-XXXXXXXX with the exact values you saved earlier</span>:
<pre lang="bash">sudo euca-bundle-image -i image.img --kernel eki-XXXXXXXX --ramdisk eri-XXXXXXXX
sudo euca-upload-bundle -b mybucket -m /tmp/image.img.manifest.xml
sudo euca-register mybucket/image.img.manifest.xml</pre>
The resulting output (emi-XXXXXXXX) will be the ID of the registered machine image.
