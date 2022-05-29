---
layout: post
status: publish
published: true
title: 'Setting Up Jekyll to run as a systemd service -- with gotchas solved'
date: '2022-05-28 00:10:29 -0600'
categories:
- code
- blog
tags: 
- jekyll
- troubleshooting
- systemd
- services
comments: []
---

## Setting Up Jekyll to run as a systemd service -- with gotchas solved

When running a [Jekyll](https://jekyllrb.com "Jekyll website") site, you will want to run the jekyll server as a systemd service.  This allows Jeyll to service requests in the background and for you to log out of the system.  This is in general how all servers run and work--using systemd `.service` files.

You can create a `.service` file by manually creating the service file using `vim` or `nano`: `sudo vim /etc/systemd/system/jekyll.service`.  You could place this in `/lib/systemd/system/jekyll.service` but it is generally convention to place user created service files in `/etc/systemd/system`. An example of the service file would look like the following:

```bash
[Unit]
Description=Jekyll service
After=syslog.target
After=network.target

[Service]
# Name of the user account that is running the Jekyll server
User=controller
Type=simple
# Location (source) of the markdown files to be rendered
ExecStart=/usr/local/bin/jekyll serve --source "/home/controller/forge" --host 127.0.0.1 --port 4000
Restart=always
StandardOutput=journal
StandardError=journal
SyslogIdentifier=jekyll

[Install]
WantedBy=multi-user.target
```

You would issue the enable and start commands for the service after saving and exiting from `vim`:

* `sudo systemctl enable jekyll.service`
* `sudo systemctl start jekyll.service`
* `sudo systemctl status jekyll.service`
  * This will print out the current status of the service
  * Which generate a new error that is not generated when you manually run the `jekyll serve` command...


```bash
May 29 04:08:41 jekyll[26701]: Configuration file: /home/controller/forge/_config.yml
May 29 04:08:41 jekyll[26701]:             Source: /home/controller/forge
May 29 04:08:41 jekyll[26701]:        Destination: /_site
May 29 04:08:41 jekyll[26701]:  Incremental build: disabled. Enable with --incremental
May 29 04:08:41 jekyll[26701]:       Generating...
May 29 04:08:41 jekyll[26701]:        Jekyll Feed: Generating feed for posts
May 29 04:08:42 jekyll[26701]:                     ------------------------------------------------
May 29 04:08:42 jekyll[26701]:       Jekyll 4.2.2   Please append `--trace` to the `serve` command
May 29 04:08:42 jekyll[26701]:                      for any additional information or backtrace.
May 29 04:08:42 jekyll[26701]:                     ------------------------------------------------
May 29 04:08:42 jekyll[26701]: /usr/lib/ruby/2.7.0/fileutils.rb:250:in `mkdir': Permission denied @ dir_s_mkdir - /_site (Errno::EACCES)
May 29 04:08:42 jekyll[26701]:         from /usr/lib/ruby/2.7.0/fileutils.rb:250:in `fu_mkdir'
```

Why is there a File Permission denied error?  Well, it has to do with the concept of a systemd service file.  Think of it this way, when you run the `jekyll serve` command from the command line you are already in the correct directory to have jekyll render to html files.  There are no permission issues.  

But when you start the `jekyll serve` command via a systemd service that starts at boot time, even though you start the program as the user -- in this case Controller -- you are not starting the `jekyll serve` command in the directory where the markdown files to be rendered are -- hence the permission denied error.  What is the solution? See the addition below to my `jekull.service` file.  You need to add a `WorkingDirectory` value to tell systemd start all activities for the Service in that defined directory.  Then your user will have the proper permissions for running the `jekyll serve` command.

```bash
[Unit]
Description=Jekyll service
After=syslog.target
After=network.target

[Service]
# Added solution -- add WorkingDirectory to directory where you clone your markdown files for Jekyll to render
WorkingDirectory=/home/controller/forge
# Name of the user account that is running the Jekyll server
User=controller
Type=simple
# Location (source) of the markdown files to be rendered
ExecStart=/usr/local/bin/jekyll serve --source "/home/controller/forge" --host 127.0.0.1 --port 4000
Restart=always
StandardOutput=journal
StandardError=journal
SyslogIdentifier=jekyll

[Install]
WantedBy=multi-user.target
```

### Conclusion

This took a little bit of detective work, but in the end it wasn't a Jekyll problem, it was a Path problem, one of the 3Ps that haunts us all-- Path, Permissions, and dePendencies.
