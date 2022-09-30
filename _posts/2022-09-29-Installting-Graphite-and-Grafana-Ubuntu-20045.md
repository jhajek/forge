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

### APT package versions - Jammy Ubuntu 21.04.1

* [graphite-api](https://packages.ubuntu.com/jammy/graphite-api "graphite api jammy apt webpage") = 1.1.3-6
* [carbon](https://packages.ubuntu.com/jammy/graphite-carbon "carbon jammy apt webpage") = 1.1.7-1
* [whisper](https://packages.ubuntu.com/jammy/python3-whisper "whisper jammy apt webpage") = 1.1.4-2.1
* [gunicorn3](https://packages.ubuntu.com/jammy/gunicorn "gunicorn3 jammy apt webpage") = 20.1.0-2

## Install Options

```bash
# Install Python dependencies
sudo apt-get update
sudo apt-get install -y python3-dev python3-setuptools python3-pip
# via PIP
python3 -m pip install graphite-api carbon whisper gunicorn
```

```bash
# Install carbon, whisper, graphite-api, and gunicorn3
sudo apt-get install -y python3-whisper graphite-carbon graphite-api python3-gunicorn
```

### Check Graphite Services' Status

```bash
# Commands to enable the services
sudo systemctl enable carbon-cache
# @1 because you can have multiple relay daemons
sudo systemctl enable carbon-relay@1
sudo systemctl enable graphite-api

# Commands to start the services
sudo systemctl start carbon-cache
# @1 because you can have multiple relay daemons
sudo systemctl start carbon-relay@1
sudo systemctl start graphite-api

# Commands to check the services' status
sudo systemctl status carbon-cache
# @1 because you can have multiple relay daemons
sudo systemctl status carbon-relay@1
sudo systemctl status graphite-api
```

## Grafana

```bash
# Steps needed to be included in install-grafana.sh
sudo apt-get install -y adduser libfontconfig1
wget https://dl.grafana.com/oss/release/grafana_9.1.6_amd64.deb
sudo dpkg -i grafana_9.1.6_amd64.deb
```
