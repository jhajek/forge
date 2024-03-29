---
layout: post
status: publish
published: true
title: 'Using Packer Proxmox-iso Template SSH Waiting Connection Problem Solved'
date: '2020-12-19 00:08:52 -0600'
tags: 
- packer
- proxmox
- qemu_agent
- ssh
comments: []
---

## TL/DR

This is a hint to clarify the missing parts in the Packer Proxmox-iso template creation documentations using Packer 1.6.6 and Proxmox VE 6.3-2.

## Introduction

Recently I have started using [Proxmox](https://proxmox.com/en/ "Proxmox website"), which is an AGPL based software version of managed KVM.  This is similar to Microsoft Hyper-V or VMware. [Packer](https://packer.io "Packer website") is a tool from [Hashicorp](https://hashicorp.com "hashicorp website") that allows one to automate an installation of an operating system.  Packer then allows you to generate artifacts that can be reused, audited, and version controlled.  

Using the Packer [Proxmox iso template creator](https://www.packer.io/docs/builders/proxmox/iso "Packer Promox iso Builder webpage") templates can be generated that can be cloned functioning operating system instances.

Using [Packer version 1.6.6](https://www.packer.io/downloads "Packer download webpage")

## Solution

In the preseed.cfg file used in Packer, Preseed needs to include/install `qemu-guest-agent`

```bash

# Individual additional packages to install
d-i pkgsel/include string build-essential ssh curl rsync git unzip wget vim openssh-server software-properties-common qemu-guest-agent

```

You also need to add the KV pair to your JSON builder so that the qemu guest agent can be used to complete the creation of the template.

```bash
      "qemu_agent": true,
```

Without that package and that KV pair you will receive this error message (with debugging on PACKER_LOG=1) and the packer build will never complete.

```text
2020/12/19 05:53:37 packer-builder-proxmox-iso plugin: [DEBUG] Error getting SSH address: 500 QEMU guest agent is not running
2020/12/19 05:53:45 packer-builder-proxmox-iso plugin: [DEBUG] Error getting SSH address: 500 QEMU guest agent is not running
```

```text
ubuntu-18045-user1: output will be in this color.

==> ubuntu-18045-user1: Creating VM
==> ubuntu-18045-user1: No VM ID given, getting next free from Proxmox
==> ubuntu-18045-user1: Starting VM
==> ubuntu-18045-user1: Starting HTTP server on port 9004
==> ubuntu-18045-user1: Waiting 10s for boot
==> ubuntu-18045-user1: Typing the boot command
==> ubuntu-18045-user1: Waiting for SSH to become available...
```

```text
2020/12/19 05:53:37 packer-builder-proxmox-iso plugin: [DEBUG] Error getting SSH address: 500 QEMU guest agent is not running
2020/12/19 05:53:45 packer-builder-proxmox-iso plugin: [DEBUG] Error getting SSH address: 500 QEMU guest agent is not running
2020/12/19 05:53:50 packer-builder-proxmox-iso plugin: [INFO] Attempting SSH connection to 192.168.1.239:22...
2020/12/19 05:53:50 packer-builder-proxmox-iso plugin: [DEBUG] reconnecting to TCP connection for SSH
2020/12/19 05:53:50 packer-builder-proxmox-iso plugin: [DEBUG] handshaking with SSH
2020/12/19 05:53:50 packer-builder-proxmox-iso plugin: [DEBUG] handshake complete!
2020/12/19 05:53:50 packer-builder-proxmox-iso plugin: [INFO] no local agent socket, will not connect agent
2020/12/19 05:53:50 packer-builder-proxmox-iso plugin: Running the provision hook
==> ubuntu-18045-user1: Connected to SSH!
==> ubuntu-18045-user1: Stopping VM
==> ubuntu-18045-user1: Converting VM to template
2020/12/19 05:53:52 packer-builder-proxmox-iso plugin: template_id: 101
2020/12/19 05:53:57 [INFO] (telemetry) ending proxmox-iso
Build 'ubuntu-18045-user1' finished after 9 minutes 34 seconds.
```

## Working Packer Proxmox-iso Template
{%raw%}
```json

{
  "variables": {
  "headless-val": "false",
  "prxmx-url": "https://IP-HERE:8006/api2/json",
  "ip":"IP-of-your packer system",
  "storagepool":"disk1",
  "storagepooltype": "lvm",
  "vmname":"ubuntu-18045",
  "uname": "username@pam",
  "password": "changeme"
},
  "builders": [
    {
      "name": "{{ user `vmname` }}",
      "vm_name": "{{ user `vmname` }}",
      "type": "proxmox-iso",
      "proxmox_url": "{{user `prxmx-url`}}",
      "insecure_skip_tls_verify": true,
      "username": "{{ user `uname` }}",
      "password": "{{ user `password` }}",
      "node": "proxmonster",
      "boot_command": [
        "<esc><wait>",
        "<esc><wait>",
        "<enter><wait>",
        "/install/vmlinuz<wait>",
        " auto<wait>",
        " console-setup/ask_detect=false<wait>",
        " console-setup/layoutcode=us<wait>",
        " console-setup/modelcode=pc105<wait>",
        " debconf/frontend=noninteractive<wait>",
        " debian-installer=en_US<wait>",
        " fb=false<wait>",
        " initrd=/install/initrd.gz ipv6.disable=1<wait>",
        " kbd-chooser/method=us<wait>",
        " keyboard-configuration/layout=USA<wait>",
        " keyboard-configuration/variant=USA<wait>",
        " locale=en_US<wait>",
        " netcfg/get_domain=vm<wait>",
        " grub-installer/bootdev=/dev/vda<wait>",
        " netcfg/get_hostname=prxmx25<wait>",
        " noapic<wait>",
        " preseed/url={{ .HTTPIP }}:{{ .HTTPPort }}/preseed/preseed-prxmx.cfg<wait>",
        " -- <wait>",
        "<enter><wait>"
      ],
      "boot_wait": "10s",
      "http_directory" : ".",
      "http_port_min" : 9001,
      "http_port_max" : 9050,     
      "iso_file": "local:iso/ubuntu-18.04.5-server-amd64.iso",
      "iso_checksum": "sha256:8c5fc24894394035402f66f3824beb7234b757dd2b5531379cb310cedfdf0996",
      "iso_storage_pool": "local",
      "os": "l26",
      "memory": 2048,
      "template_name": "Ubuntu18045",
      "scsi_controller":"virtio-scsi-pci",
      "qemu_agent": true,
      "boot": "order=virtio0;ide2",
      "network_adapters": [
        {
          "bridge": "vmbr0",
          "model": "virtio",
          "firewall": false
        }
      ],
      "disks": [
        {
          "type": "virtio",
          "disk_size": "15G",
          "storage_pool": "{{ user `storagepool` }}",
          "storage_pool_type": "{{ user `storagepooltype` }}"
        }
      ],      
      "ssh_username": "vagrant",
      "ssh_password": "vagrant",
      "ssh_port": 22,
      "ssh_wait_timeout": "10000s",
      "http_bind_address":"{{user `ip`}}",
      "unmount_iso": true
      }],
      
      "provisioners": [
        {
          "type": "shell",
        "execute_command" : "echo 'vagrant' | {{ .Vars }} sudo -E -S sh '{{ .Path }}'", 
          "script": "../scripts/post_install_prxmx.sh"
        }
      ]
}
```
{%endraw%}

## Working Preseed File

```bash

# inspired by Hashicorp sample 
# https://www.packer.io/guides/automatic-operating-system-installs/preseed_ubuntu
# Preseeding only locale sets language, country and locale.
d-i debian-installer/locale string en_US

# Keyboard selection.
d-i console-setup/ask_detect boolean false
d-i keyboard-configuration/xkb-keymap select us

choose-mirror-bin mirror/http/proxy string

### Clock and time zone setup
d-i clock-setup/utc boolean true
d-i time/zone string UTC

# Avoid that last message about the install being complete.
d-i finish-install/reboot_in_progress note

# This is fairly safe to set, it makes grub install automatically to the MBR
# if no other operating system is detected on the machine.
d-i grub-installer/only_debian boolean true

# This one makes grub-installer install to the MBR if it also finds some other
# OS, which is less safe as it might not be able to boot that other OS.
d-i grub-installer/with_other_os boolean true

### Mirror settings
# If you select ftp, the mirror/country string does not need to be set.
d-i mirror/country string manual
d-i mirror/http/directory string /ubuntu/
d-i mirror/http/hostname string archive.ubuntu.com
d-i mirror/http/proxy string

### Partitioning
d-i partman-auto/method string lvm

# This makes partman automatically partition without confirmation.
d-i partman-lvm/confirm boolean true
d-i partman-lvm/device_remove_lvm boolean true
d-i partman-auto/choose_recipe select atomic

d-i partman/confirm_write_new_label boolean true
d-i partman/confirm_nooverwrite boolean true
d-i partman/choose_partition select finish
d-i partman/confirm boolean true

# Write the changes to disks and configure LVM?
d-i partman-lvm/confirm boolean true
d-i partman-lvm/confirm_nooverwrite boolean true
d-i partman-auto-lvm/guided_size string max
### Account setup
d-i passwd/user-fullname string vagrant
d-i passwd/user-uid string 1000
d-i passwd/user-password password vagrant
d-i passwd/user-password-again password vagrant
d-i passwd/username string vagrant

# The installer will warn about weak passwords. If you are sure you know
# what you're doing and want to override it, uncomment this.
d-i user-setup/allow-password-weak boolean true
d-i user-setup/encrypt-home boolean false

### Package selection
tasksel tasksel/first standard
d-i pkgsel/include string openssh-server build-essential
d-i pkgsel/install-language-support boolean false

# disable automatic package updates
d-i pkgsel/update-policy select none
d-i pkgsel/upgrade select full-upgrade

# Individual additional packages to install
d-i pkgsel/include string build-essential ssh curl rsync git unzip wget vim openssh-server software-properties-common qemu-guest-agent

# Go grub, go!
d-i grub-installer/only_debian boolean true

d-i finish-install/reboot_in_progress note

```
