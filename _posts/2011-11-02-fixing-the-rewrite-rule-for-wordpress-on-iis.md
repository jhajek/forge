---
layout: post
status: publish
published: true
title: Fixing the rewrite rule for Wordpress on IIS
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
date: '2011-11-02 10:02:41 -0500'
date_gmt: '2011-11-02 16:02:41 -0500'
tags: []
comments: []
---
<p>When using Wordpress on IIS - changing the Settings > Permalinks > Month and Name failed.  It was not obvious at first since all the articles were rendered successfully.  If you look at the bottom of the Permalinks screen it gives you instructions on how to modify your <a href="http://en.wikipedia.org/wiki/Web.config">web.config</a> to make sure it can render Month and Name URLs.  </p>
<p>For example by default Wordpress will render all articles with a simple id:  <code>http://blog.sat.iit.edu/?p=123</code></p>
<p>But you want the setting to be Month and Name: <code>http://blog.sat.iit.edu/2011/11/sample-post/</code></p>
<p>The instructions are not clear where to add the entry to your <a href="http://en.wikipedia.org/wiki/Web.config">web.config</a>.  It has been provided for you, an entire <a href="http://en.wikipedia.org/wiki/Web.config">web.config</a> that will render your permalinks as Month and Name. </p>
<pre lang="XML" line="1">
<?xml version="1.0" encoding="UTF-8"?><br />
<configuration><br />
    <system.webServer><br />
        <defaultDocument><br />
            <files><br />
                <clear /><br />
                <add value="index.php" /><br />
                <add value="Default.htm" /><br />
                <add value="Default.asp" /><br />
                <add value="index.htm" /><br />
                <add value="index.html" /><br />
                <add value="iisstart.htm" /><br />
            </files><br />
		</defaultDocument><br />
		<rewrite><br />
		  <rules><br />
		    <rule name="wordpress" patternSyntax="Wildcard"><br />
		      <match url="*" /><br />
		        <conditions><br />
			  <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" /><br />
			  <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" /><br />
			</conditions><br />
		      <action type="Rewrite" url="index.php" /><br />
		     </rule><br />
		   </rules><br />
		 </rewrite><br />
    </system.webServer><br />
</configuration><br />
</pre></p>
