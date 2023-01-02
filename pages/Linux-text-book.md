---
layout: page
status: publish
published: true
title: Intro to Linux Textbook
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
date: '2020-12-09 11:03:41 -0500'
---

Starting in 2015 I began to long commute back and forth to work via the train.  While I had free time I began to take my notes I had collected over the years and began to craft some deeper tutorials. Those tutorials culminated into chapters, which eventually became a book.

I present to you: **Understanding Free and Opensource Operating Systems, The Technology and Philosophy Of - Part I**

The book has:

* 14 chapters
  * Covering concepts of Opensource and Free Software
  * Covering detailed hands on commandline work (vi, package mangers, path, etc, etc.)
* Quizzes and exercises provided
* 500+ pages
* Since it is about opensource -- the book itself is opensource
  * [https://github.com/jhajek/Linux-text-book-part-1/releases](https://github.com/jhajek/Linux-text-book-part-1/releases "URL to book GitHub project")
  * PDF and epub available and update frequently
  * Make a pull request to update some content or contribute
  
{% for post in site.categories.textbook %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
