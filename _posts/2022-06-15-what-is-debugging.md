---
layout: post
status: publish
published: true
title: 'What is debugging?'
tags: 
- debugging
- cloud
- teaching
comments: []
---

I define troubleshooting/debugging as not making the problem go away, but asking questions of a system until the answer reveals itself and you have gained a deeper fundamental understanding of your system. But how can you begin to ask questions, especially when you are just starting?  Debugging can look like magic, but just like all magic tricks once you know the trick the illusion falls apart.

### 3P's Framework

A useful framework I have arrived at for debugging is the 3P's:

* Path
  * If you get an error message telling you that file not found or path does not exist double check your path. It the absolute path correct? Is it a relative path problem? Are you on the wrong level?
* Permission
  * Every file has permission on what is allowed to be done with it based on a simple access control of read write and execute. Maybe you don’t have permission to write and therefore can’t delete a file. Perhaps the file is owned by someone else and they didn’t give you permission? Check permissions via `ls -la` for instance.
* dePendencies
  * Are all the correct software dependencies installed? Perhaps you are missing a library or have an incompatible version that is preventing a tool from running? Often a system library or language dependency is missing.
* All else fails and you still have a problem, see if it is a full moon outside
