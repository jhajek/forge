---
layout: post
status: publish
published: true
title: How to escape an at-sign @ in Pandoc markdown
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1580
wordpress_url: https://forge.sat.iit.edu/?p=1580
date: '2015-10-31 23:46:50 -0500'
date_gmt: '2015-11-01 04:46:50 -0600'
categories:
- Pandoc
- Textbook
tags:
- linux
- textbook
- self-publish
- errors
- pandoc
comments: []
---
<p>Using pandoc markdown to write a text book - I came across a table from Wikipedia I wanted to include.<br />


--------- ------------------------------------------------- -------------------
@yearly    Run once a year at midnight of January 1st         ```0 0 1 1 *```
@monthly   Run once a month, midnight 1st day of the month.   ```0 0 1 * *```
@weekly    Run once a week at midnight on Sunday morning      ```0 0 * * 0```
@daily     Run once a day at midnight                         ```0 0 * * *```
@hourly    Run once an hour at the beginning of the hour.     ```0 * * * *```
@reboot    Run at startup.                                       @reboot
--------- ------------------------------------------------- -------------------

<p>But using <a href="http://pandoc.org/">Pandoc</a> markdown syntax on Windows (you need to use the <a href="http://miktex.org/">MiKTeX</a> Latex PDF engine) causes a crash when encountering <code>@yearly</code>. Or so I thought. When I changed the code to look like this no crashing:<br />
<code></p>
<pre>
Entry Description Equivalent to<br />
--------- ------------------------------------------------- -------------------<br />
&amp;&#35;64&#59;yearly    Run once a year at midnight of January 1st         ```0 0 1 1 *```<br />
&amp;&#35;64&#59;monthly   Run once a month, midnight 1st day of the month.   ```0 0 1 * *```<br />
&amp;&#35;64&#59;weekly    Run once a week at midnight on Sunday morning      ```0 0 * * 0```<br />
&amp;&#35;64&#59;daily     Run once a day at midnight                         ```0 0 * * *```<br />
&amp;&#35;64&#59;hourly    Run once an hour at the beginning of the hour.     ```0 * * * *```<br />
&amp;&#35;64&#59;reboot.   Run at startup                                       &amp;&#35;64&#59;reboot<br />
--------- ------------------------------------------------- -------------------<br />
</pre><br />
</code></p>
<p>The @ sign is a reserved LaTeX word and causes a crash.</p>
<p>The solution is to use the HTML ascii code for an @ sign which is: <code>&amp;&#35;64&#59;</code> which will render correctly as ascii and unconfuse the LaTeX compiler and remove the error.</p>
