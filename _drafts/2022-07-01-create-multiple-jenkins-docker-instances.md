---
layout: post
status: publish
published: true
title: 'How to create multiple Jenkins Docker instances on a single server'
tags: 
- jenkins
- docker
- ubuntu
- howto
comments: []
---


https://www.packer.io/plugins/builders/hyperv/iso

Packer --version 1.7.4

https://github.com/kreuzwerker/terraform-provider-docker

https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/network

Assuming you created a network as follows

```bash
#!/bin/bash
docker network create foo
# prints the long ID
87b57a9b91ecab2db2a6dbf38df74c67d7c7108cbe479d6576574ec2cd8c2d73
```

```bash
# You provide the definition for the resource as follows

resource "docker_network" "foo" {
  name = "foo"
}
```

```bash
# Set the required provider and versions
# Jenkins Docker
# https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/reso$urces/network

variable "network_name" {
  type    = string
  default = "jenkins-net"
}

variable "instance_name" {
  type    = string
  default = "jenkins"
}

variable "internal_port" {
  type   = number
  default = 8080
}

variable "external_port" {
  type = number
  default = 8080
}

variable "public_ip" {
   type = string
   default = "192.168.172.30"
}

terraform {
  required_providers {
    # We recommend pinning to the specific version of the Docker Provider you're using
    # since new versions are released frequently
    docker = {
      source  = "kreuzwerker/docker"
      version = "2.17.0"
    }
  }
}

resource "docker_network" "private_network" {
  name = var.network_name
}

# Configure the docker provider
provider "docker" {
}

# Create a docker image resource
# -> docker pull nginx:latest
resource "docker_image" "jenkins" {
  name         = "jenkins/jenkins:lts"
  keep_locally = true
}

# Create a docker container resource
# -> same as 'docker run --name nginx -p8080:8080 -d nginx:latest'
resource "docker_container" "jenkins" {
  name  = var.instance_name
  image = docker_image.jenkins.latest
  networks_advanced {
    name = var.network_name
  }

  ports {
    external = var.external_port
    internal = var.internal_port
    ip       = var.public_ip
  }
}
```