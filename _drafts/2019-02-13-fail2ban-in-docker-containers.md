---
layout: post
status: publish
published: true
title: 'Fail2ban inside of a Docker Container'
author:
  display_name: Jeremy Hajek
  login: jhajek
  email: hajek@.iit.edu
author_login: jhajek
author_email: hajek@.iit.edu
date: '2019-02-13 09:12:52 -0600'
date_gmt: '2019-02-13 15:14:28 -0600'
categories:
- software
- cloud
- containers
tags: 
- docker
- fail2ban
- networking
comments: []
---

# fail2ban

Using [fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page "fail2ban website") for blocking brute force SSH connection attempts has been a go-to tool for nearly a decade now.  Any server that has public facing internet connections needs this tool or a similar one or else your systems could be in danger.   

The [fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page "fail2ban website") product also has abilites to monitor and block failed log in attempts for other software.

## The Database

This project we are embarking on uses [Mariadb 10.3](https://mariadb.com/ "mariadb website") which is a fork and drop in replacement of [Oracle's MySQL](https://www.oracle.com/mysql/ "oracle mysql website").  Simple enough.  Fail2ban will protect us from SSH bruteforce attacks by default, but we need to configure it to protect us from drive brute force connection attempts.  To complicate this, the mariadb server is running in a Docker container with its internet port forwarded externally.    So where does fail2ban need to run?  On the host or in the contianer?

## Docker and the Diagram

Fail2ban works by scannig for failed login attempts and simply bans that IP after a certain number of strikes or fail logins per a defined time period. This makes use of the log and error files located in ```/var/log```.  But when the database is running in a [Docker container](https://www.docker.com/ "Docker website") there is no log file on the host system because the applicaiton is in the container.  Whereas fail2ban is running onthe host system and not inside of the container.   To complicate this from a visualization perspective, the Docker process is forwarding port from the container to the host system (yes I have hardened passwords and remote root login denied.). 

https://serverfault.com/questions/253452/how-do-i-setup-monitoring-of-mysql-with-fail2ban

How do I setup monitoring of MySQL with Fail2ban?

http://fail2sql.sourceforge.net/

Fail2SQL - An SQL logger for Fail2Ban

https://stackoverflow.com/questions/37996088/secure-server-with-fail2ban-and-docker
Secure server with Fail2ban and Docker

```docker run -v /path/in/host:/var/log/nginx/access.log nginx``` as an example.

But how to get fail2ban access to the logs, and though you can execute the ```sudo docker logs container-name``` command and see the logs.

