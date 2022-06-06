---
layout: post
status: publish
published: true
title: 'Ubuntu Server forgetting IPv4'
date: '2022-06-02 11:14:59 -0600'

tags: 
- ubuntu
- troubleshooting
- server
- 20.04
- ipv4
- ipv6
comments: []
---

## Ubuntu Server forgetting IPv4

For years now I have been using Ubuntu and Ubuntu server.  From 12.04 and each LTS moving forward I have hitched my wagon to Ubuntu Server.  Though over the years there has been one interesting issue.  I haven't quite nailed it down.  I don't think this is tied to a particular version or even a particular service file.  I feel this is a networking issue.  Half of me wants to reboot the system or reinstall and be down with it.  The other half wants to stay and debug the problem in situ.  What is the problem then?

### Networking Problem

The issue is that for a reason, a service will stop listening on IPv4 but continuing to listen on IPv6.  The service still runs, just not on the IP version I expect. In this case I am using [Riemann.io](http://riemann.io "Riemann.io web page").  Which listens in port 5555/tcp (more or less).  

```bash
# ss -ln output abridged
udp UNCONN 0 0      [::ffff:10.0.0.36]:5555     *:*          uid:996 ino:27492 sk:64 v6only:0 <->
tcp LISTEN 0 128    0.0.0.0:22                  0.0.0.0:*    ino:24661 sk:68 <->
tcp LISTEN 0 50     [::ffff:10.0.0.36]:5555     *:*          uid:996 ino:27479 sk:6a v6only:0 <->
tcp LISTEN 0 50     [::ffff:10.0.0.36]:5556     *:*          uid:996 ino:27474 sk:6b v6only:0 <->
tcp LISTEN 0 50     [::ffff:127.0.0.1]:5557     *:*          uid:996 ino:27472 sk:6c v6only:0 <->
tcp LISTEN 0 128    [::]:22                     [::]:*       ino:24672 sk:6d v6only:1 <->
```

So a few solutions to try (reboot and service restart service doesn't change)

There is one option to disable ipv6, which can be done [two ways](https://askubuntu.com/questions/1300253/disable-ipv6-on-ubuntu-20-04 "Two ways to disable ipv6 web page")

```bash
# Edit /etc/sysctl.conf and add the below
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
# Afterwards sysctl -p
```

You can then reboot and check if the system has ipv6 disabled with the following command:

```bash
ip a sh
```

Sometimes this is overridden or *doesn't work*.  So the other way to do this is to modify the GRUB boot loader and tell the Linux kernel not to load the IPv6 stack.  This will require editing `/etc/default/grub` (assuming Ubuntu). You will be adding/appending the value `ipv6.disable=1` to two values:

* `GRUB_CMDLINE_LINUX_DEFAULT=" ipv6.disable=1"`
* `GRUB_CMDLINE_LINUX="ipv6.disable=1"`
* Issue the grub update command and reboot: `sudo update-grub`

Run the `ip a sh` command again and you will see your IPv6 stack disabled.  Your network interface outputs and the ipv6 will be missing. Now your Riemann service is listening again on IPv4, but why this happened will bug me until it happens again.

```bash
tcp LISTEN 0 50 10.0.0.36:5555 0.0.0.0:*
tcp LISTEN 0 50 10.0.0.36:5556 0.0.0.0:*
```

### Conclusion

This took a bit of time to figure out and in truth I didn't really solve, but mitigated the problem.  That is the contention of the IT Admin - get it running again or take the time to debug and find the core issue.
