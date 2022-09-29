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
* Carbon
  * [Carbon](https://github.com/graphite-project/carbon "carbon webpage") has two service daemons, Carbon-Relay and Carbon-Cache that cache received metrics and then write them to storage
* Whisper
  * [Whisper](https://github.com/graphite-project/whisper "whisper webpage") is the flat-file metric optimized storage format that goes along with Graphite.

## Install Graphite

```bash
# Install Python dependencies
sudo apt-get update
sudo apt-get install -y python3-dev python3-setuptools python3-pip
```

```bash
# Install carbon and whisper

```


## Grafana

