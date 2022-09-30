---
layout: post
status: publish
published: true
title: 'Installing Grafana and Graphite on Ubuntu Server 20.04.5'
categories:
- software
- Ubuntu
- monitoring
- configuration
tags: 
- linux
- ubuntu
- monitoring
- observability
comments: []
---

Tried and true Grafana and Graphite as a metrics collection tool and a graphing tool. There has been some bugs along the way, but I think I can detail it as most of the process works out of the box, or out of the `apt` package you can say.

Assuming a full [Ubuntu Server 20.04.5](https://mirrors.edge.kernel.org/ubuntu-releases/20.04.5/ "Ubuntu mirrors") install. Now we begin to install packages, Grafana and Graphite.

## Graphite

Graphite is a bit of a trick to install as it is really 3 parts, and each part has Python dependencies.

* Graphite-web
  * A Django-based web application that renders graphs and dashboards
    * This is not used as Grafana has replaced the need for it
  * We want just the [Graphite-api](https://pypi.org/project/graphite-api/ "graphite api webpage") so Grafana can interact with Whisper
* Carbon
  * [Carbon](https://github.com/graphite-project/carbon "carbon webpage") has two service daemons, Carbon-Relay and Carbon-Cache that cache received metrics and then write them to storage
* Whisper
  * [Whisper](https://github.com/graphite-project/whisper "whisper webpage") is the flat-file metric optimized storage format that goes along with Graphite.

## Install Graphite

Two ways to install the packages needed. We can use the Ubuntu Package Manager, `apt`, to install them via apt-get or we can install Python3 PIP package manager and install them via PIP3. The Ubuntu `apt` package manager has simplicity because it is built into the Operating System, but the main thing to consider here is software versions. The Python package manager is independent of the OS so it can update software on its own schedule--separate from the OS. Let us compare what versions are available.

Without the Django app portion of graphite-web, we will need gunicorn3 to be installed to serve the graphite-api application requests.

### Python3 PIP

* [graphite-api](https://pypi.org/project/graphite-api/ "graphite api pypi webpage") = 1.1.3
* [carbon](https://pypi.org/project/carbon/ "carbon pypi webpage") = 1.1.10
* [whisper](https://pypi.org/project/whisper/ "whisper pypi webpage") = 1.1.0
* [gunicorn3](https://pypi.org/project/gunicorn/ "gunicorn pypi webpage") = 20.1.0

### APT package versions - Focal Ubuntu 20.04.5

* [graphite-api](https://packages.ubuntu.com/focal/graphite-api "graphite api focal apt webpage") = 1.1.3-5
* [carbon](https://packages.ubuntu.com/focal/graphite-carbon "carbon apt focal webpage") = 1.1.4-2
* [whisper](https://packages.ubuntu.com/focal/python3-whisper "whisper focal apt webpage") = 1.1.4-2
* [gunicorn3](https://packages.ubuntu.com/focal/gunicorn "gunicorn focal apt webpage") = 20.0.4-3

### APT package versions - Jammy Ubuntu 22.04.1

* [graphite-api](https://packages.ubuntu.com/jammy/graphite-api "graphite api jammy apt webpage") = 1.1.3-6
* [carbon](https://packages.ubuntu.com/jammy/graphite-carbon "carbon jammy apt webpage") = 1.1.7-1
* [whisper](https://packages.ubuntu.com/jammy/python3-whisper "whisper jammy apt webpage") = 1.1.4-2.1
* [gunicorn3](https://packages.ubuntu.com/jammy/gunicorn "gunicorn3 jammy apt webpage") = 20.1.0-2

## Install Options

```bash
# Install Python dependencies
sudo apt-get update
sudo apt-get install -y python3-dev python3-setuptools python3-pip
# Install carbon, whisper, graphite-api, and gunicorn3 via PIP
python3 -m pip install gunicorn graphite-api carbon whisper
```

```bash
# Install carbon, whisper, graphite-api, and gunicorn3 via APT
sudo apt-get install -y gunicorn python3-whisper graphite-carbon graphite-api
```

Which one to choose?  Well it turns out that the APT package for `graphite-api` on Ubuntu 20.04, 1.1.3-5, has some dependency problems. To get around this, we can install the `graphite-api` via Python3 PIP. In addition, the `Carbon` package in APT is missing a `relay-rules.conf` file, needed for the `carbon-relay service to start`. We can retrieve the missing file from the projects [GitHub project](https://github.com/graphite-project/carbon/blob/master/conf/relay-rules.conf.example "GitHUb project website for graphite").

### Automated Instructions

```bash
# Install Python dependencies
sudo apt-get update
# Graphite-api -- turns out the problem is the default package in Ubuntu 20.04 repository for graphite-api 1.1.3-5 
# https://bugs.launchpad.net/ubuntu/+source/graphite-api/+bug/1879148
# The solution was to grab the fixed package from the next version of Ubuntu 20.10, codenamed Groovy Gorilla
# https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=954600
# Is this normal?  No but its only for the Ubuntu 20.04 Focal distro that this fix wasn't back ported to
# https://packages.ubuntu.com/groovy/graphite-api
# https://packages.ubuntu.com/focal/graphite-api

# Retrieve the package Python-Structlog that needs to be backported to Ubuntu 20.04 from the 20.10 repo
wget http://archive.ubuntu.com/ubuntu/pool/universe/p/python-structlog/python3-structlog_20.1.0-1_all.deb
sudo dpkg -i ./python3-structlog_20.1.0-1_all.deb
# Retrieve the graphite-api package that needs to be backported to Ubuntu 20.04 from the 20.10 repo
wget http://archive.ubuntu.com/ubuntu/pool/universe/g/graphite-api/graphite-api_1.1.3-6_all.deb
sudo dpkg -i ./graphite-api_1.1.3-6_all.deb

# Install carbon, whisper, graphite-api, and gunicorn3 via APT
sudo apt-get install -y gunicorn python3-whisper graphite-carbon
# Ubuntu Focal 20.04 Carbon package doesn't contain this relay-rules.conf file need 
# to retrieve it via wget and manually copy it into /etc/carbon, otherwise 
# carbon-relay@1 service won't start
wget https://raw.githubusercontent.com/graphite-project/carbon/master/conf/relay-rules.conf.example
sudo mv -v ./relay-rules.conf.example /etc/carbon/relay-rules.conf
sudo systemctl daemon-reload

###################################
# Check Graphite Services' Status #
###################################
# Commands to enable the services (not needed on Ubuntu but good habit)
sudo systemctl enable carbon-cache
# @1 because you can have multiple relay daemons
sudo systemctl enable carbon-relay@1
sudo systemctl enable graphite-api

# Commands to check the services' status
sudo systemctl status carbon-cache
# @1 because you can have multiple relay daemons
sudo systemctl status carbon-relay@1
sudo systemctl status graphite-api
```

## How to Install Grafana

[Grafana is an Open Source graphing solution](https://grafana.com/oss "Grafana webpage"). Grafana does not store any data but has interfaces to read metrics from many different time of time storage databases and traditional databases.

```bash
# Steps needed to be included in install-grafana.sh
sudo apt-get install -y adduser libfontconfig1
wget https://dl.grafana.com/oss/release/grafana_9.1.6_amd64.deb
sudo dpkg -i grafana_9.1.6_amd64.deb
```

**But** the latest Grafana versions assume you are using systemd version 247+ which supports a feature called `ProtectProc`. [This hides processes `/proc` from the services](https://www.sherbers.de/use-temporaryfilesystem-to-hide-files-or-directories-from-systemd-services "webpage for explaining ProtectProc"). Unfortunately, Ubuntu 20.04.5 supports only systemd version 245. So a simple find/replace command using `sed` to comment out this feature will be needed, or upgrade to Ubuntu 22.04.1 Jammy.

```bash
# After Grafana install
sudo sed -i 's/ProtectProc=invisible/#ProtectProc=invisible/g' /lib/systemd/system/grafana-server.service
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

## Conclusion

The steps to maintain an install of Graphite and Grafana have not aged as expected. Going back to Ubuntu 14/16 this process was simple. Overtime dependencies have creeped and new solutions have been released, though Grafana and Graphite are still well known and still widely in use. I think this is a good exercise of finding out software dependencies and how software is installed and finally showing that its a miracle that anything works.
