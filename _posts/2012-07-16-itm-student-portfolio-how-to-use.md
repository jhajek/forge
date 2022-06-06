---
layout: post
status: publish
published: true
title: 'ITM Student Portfolio: How to Use'
author:
  display_name: Jeffrey Hartmann
  login: jhartma3
  email: jhartma3@hawk.iit.edu
  url: http://www.linkedin.com/in/jeffreyhartmann
author_login: jhartma3
author_email: jhartma3@hawk.iit.edu
author_url: http://www.linkedin.com/in/jeffreyhartmann
date: '2012-07-16 12:07:52 -0500'
date_gmt: '2012-07-16 18:07:52 -0500'
tags: []
comments: []
---
<p><em><strong>This project is a work-in-progress. These notes may reflect aspects of the project which have yet to be fully realized. As the project progresses, these notes may be amended at any time.</strong></em></p>
<p>Student Portfolio is a project designed for the Information Technology and Management Department (ITM) of the Illinois Institute of Technology (IIT). The goal is to enable students to upload resumes, screenshots and relevant hyperlinks (such as links to projects as well as LinkedIn profiles) to a server and thus help students promote themselves with a link to their portfolios.</p>
<p><strong><em>The Portfolio Database</em></strong></p>
<p>For those who hold the proper credentials, the database can be found at <a href="http://diamond.sat.iit.edu/phpmyadmin/">http://diamond.sat.iit.edu/phpmyadmin/</a>. Upon successfully logging in, look for a database called &Gamma;&Ccedil;&pound;pmdb&Gamma;&Ccedil;&yen; (all lower-case).</p>
<p>The &Gamma;&Ccedil;&pound;pmdb&Gamma;&Ccedil;&yen; database consists of three tables:</p>
<ul>
<li>&Gamma;&Ccedil;&pound;<strong>profile</strong>&Gamma;&Ccedil;&yen; is a &Gamma;&Ccedil;&pound;parent&Gamma;&Ccedil;&yen; table which consists of general information about the portfolio user.
<ul>
<li>&Gamma;&Ccedil;&pound;<strong>idprofile</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; Populated automatically, this is a unique key field which identifies a profile; it also ties to the &Gamma;&Ccedil;&pound;project&Gamma;&Ccedil;&yen; table.</li>
<li>&Gamma;&Ccedil;&pound;<strong>username</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the LDAP username of the portfolio user.</li>
<li>&Gamma;&Ccedil;&pound;<strong>namefirst</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the portfolio user&Gamma;&Ccedil;&Ouml;s first name.</li>
<li>&Gamma;&Ccedil;&pound;<strong>namelast</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the portfolio user&Gamma;&Ccedil;&Ouml;s last name.</li>
<li>&Gamma;&Ccedil;&pound;<strong>urllinkedin</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the portfolio user&Gamma;&Ccedil;&Ouml;s link to the LinkedIn profile.</li>
<li>&Gamma;&Ccedil;&pound;<strong>fileresume</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the filename, including extension, of the user&Gamma;&Ccedil;&Ouml;s resume.</li>
<li>&Gamma;&Ccedil;&pound;<strong>contactemail</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the user&Gamma;&Ccedil;&Ouml;s contact e-mail address; the portfolio&Gamma;&Ccedil;&Ouml;s &Gamma;&Ccedil;&pound;Contact Me&Gamma;&Ccedil;&yen; form would not appear without it.</li>
<li>&Gamma;&Ccedil;&pound;<strong>contactmessage</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; a custom message which the user could create for the portfolio&Gamma;&Ccedil;&Ouml;s &Gamma;&Ccedil;&pound;Contact Me&Gamma;&Ccedil;&yen; form.<br /><br />
(I am open to hard-coding a brief, generic message explaining the contact form and then eliminating this field and the data-entry option on the site for this field.)</li></p>
<li>The remaining fields (&Gamma;&Ccedil;&pound;<strong>datecreated</strong>&Gamma;&Ccedil;&yen;, &Gamma;&Ccedil;&pound;<strong>dateupdated</strong>&Gamma;&Ccedil;&yen;) are populated automatically.</li><br />
</ul><br />
</li></p>
<li>&Gamma;&Ccedil;&pound;<strong>project</strong>&Gamma;&Ccedil;&yen; is a &Gamma;&Ccedil;&pound;child&Gamma;&Ccedil;&yen; table which consists of information about the portfolio user&Gamma;&Ccedil;&Ouml;s projects.
<ul>
<li>&Gamma;&Ccedil;&pound;<strong>idproject</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; Populated automatically, this is a unique key field which identifies a project.</li>
<li>&Gamma;&Ccedil;&pound;<strong>title</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the title of the project.</li>
<li>&Gamma;&Ccedil;&pound;<strong>description</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the description of the project.</li>
<li>&Gamma;&Ccedil;&pound;<strong>urlwork</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the link to the live project. <strong></strong></li>
<li>&Gamma;&Ccedil;&pound;<strong>imgleft</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the filename, including extension, of the large screenshot to the left of the portfolio.</li>
<li>&Gamma;&Ccedil;&pound;<strong>altleft</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; a description of &Gamma;&Ccedil;&pound;imglect&Gamma;&Ccedil;&yen;, a caption of a sort which appears upon mouseover of the image.</li>
<li>&Gamma;&Ccedil;&pound;<strong>imgrighttop</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the filename, including extension, of the large screenshot to the top-right of the portfolio.</li>
<li>&Gamma;&Ccedil;&pound;<strong>altrighttop</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; a description of &Gamma;&Ccedil;&pound;imgrighttop&Gamma;&Ccedil;&yen;, a caption of a sort which appears upon mouseover of the image.</li>
<li>&Gamma;&Ccedil;&pound;<strong>imgrightbottom</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the filename, including extension, of the large screenshot to the bottom-right of the portfolio.</li>
<li>&Gamma;&Ccedil;&pound;<strong>altrightbottom</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; a description of &Gamma;&Ccedil;&pound;imgrightbottom&Gamma;&Ccedil;&yen;, a caption of a sort which appears upon mouseover of the image.</li>
<li>The remaining fields (&Gamma;&Ccedil;&pound;<strong>datecreated</strong>&Gamma;&Ccedil;&yen;, &Gamma;&Ccedil;&pound;<strong>dateupdated</strong>&Gamma;&Ccedil;&yen;) are populated automatically.</li><br />
</ul><br />
</li></p>
<li>&Gamma;&Ccedil;&pound;<strong>users</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; a temporary login system, &Gamma;&Ccedil;&pound;temporary&Gamma;&Ccedil;&yen; meaning that we ultimately remove this table and replace it with an LDAP-based login system. For now it must remain in place to maintain functionality during development.
<ul>
<li>&Gamma;&Ccedil;&pound;<strong>iduser</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; Populated automatically. [Note: Review code to see whether it is in use; it ought not to be.]</li>
<li>&Gamma;&Ccedil;&pound;<strong>username</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; LDAP username</li>
<li>&Gamma;&Ccedil;&pound;<strong>password</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; MD5-encrypted LDAP password; during the development process a password would be created and then run through an online MD5-conversion website; the resulting MD5-encrypted version of the password would be pasted here.</li>
<li>The remaining fields (&Gamma;&Ccedil;&pound;<strong>datecreated</strong>&Gamma;&Ccedil;&yen;, &Gamma;&Ccedil;&pound;<strong>dateupdated</strong>&Gamma;&Ccedil;&yen;) are populated automatically.</li><br />
</ul><br />
</li><br />
</ul><br />
<strong><em>The Portfolio Itself</em></strong></p>
<p>[Documentation in Progress]</p>
<p>The URL for the Portfolio is http://diamond.sat.iit.edu/[student's LDAP username]/.</p>
<p>The portfolio is based on a template which at first consisted of HTML and jQuery. PHP and MySQL were applied so that data for the student&Gamma;&Ccedil;&Ouml;s profile and projects could be retrieved quickly instead of manually added to the HTML code, especially per project. The programming is procedural, without additional frameworks involved (unlike the supporting CMS System, which uses CodeIgniter).</p>
<p>The portfolio is available to the public, with no login.</p>
<p>Without data in the database for the student, the Portfolio begins blank. The students would add the data themselves via the Portfolio CMS System.</p>
<p>Data retrieval is based in no small part on the portion of the URL which holds the student&Gamma;&Ccedil;&Ouml;s LDAP username.</p>
<p><strong><em>The Portfolio CMS System</em></strong></p>
<p>[Documentation in Progress]</p>
<p>The URL for the Portfolio CMS System is http://diamond.sat.iit.edu/[student's LDAP username]/cms/. The portfolio owner simply adds "cms/" to the Portfolio URL and login.</p>
<p>The CMS is a PHP/MySQL-based project built via CodeIgniter (http://codeigniter.com/) The version used is 2.0.3; as of this writing CodeIgniter may be up to Version 2.1.2. Portions of the CMS are based in turn on a CodeIgniter 1.6.2 tutorial readapted for 2.0.3:┬&aacute;<a href="http://henrihnr.wordpress.com/2009/04/26/simple-crud-application/">http://henrihnr.wordpress.com/2009/04/26/simple-crud-application/</a></p>
<p>In CodeIgniter, traditional file management does not apply. CodeIgniter is a PHP framework based on the MVC (Model-View-Controller) design pattern. Rather than calling an actual file by name and extension via a URL, a call to the name of a function in a Controller generates a URL which retrieves files related to the View (HTML, CSS, etc) while, as needed, retrieving data based on a given Model. CodeIgniter has very strict, case-sensitive, file-naming and function-naming conventions.</p>
<p><strong><em>File Management and Troubleshooting</em></strong></p>
<p>[Documentation in Progress]</p>
<p>Many files and folders common to both the Portfolio and its CMS are in "/var/www/portfolio". This was done to reduce the "footprint" of each student's portfolio to be added to the server. If errors occur which are common across all portfolios, this directory is a good place to start your troubleshooting.</p>
<p>Each student portolio directory is referenced by the student's LDAP username. Thus each student's portfolio shall be found at "<strong>/var/www/[student's LDAP username]/</strong>".</p>
<p>The "cms" directory in each student portfolio folder is a CodeIgniter package, consisting of all the folders and files which drive CodeIgniter. Being a CodeIgniter package, most of the files and directories within "cms" need not and ought not to be touched. That said, the files which were created for the CMS can be found in the following directories within "<strong>.../cms/application/</strong>":</p>
<ul>
<li><strong>models</strong> (where you find the SQL statements and calls to the MySQL database)</li>
<li><strong>views</strong> (the html files which comprise the visual aspects of the CMS, which in turn summon CSS files -- many of which are in the common ".../portfolio/" directory)</li>
<li><strong>controllers</strong>
<ul>
<li>login.php and verifylogin.php<br />
(The conversion from a database-based login system to an LDAP-based login system might be applied in one or both of these files.)</li></p>
<li>project.php</li>
<li>profile.php</li><br />
</ul><br />
</li></p>
<li><strong>config</strong>
<ul>
<li>config.php</li>
<li>routes.php</li>
<li>database.php</li>
<li>autoload.php</li><br />
</ul><br />
</li><br />
</ul><br />
Besides ".../application/", ".../system/" is also a directory which comes with CodeIgniter. This directory rarely needs to be touched.</p>
<p>Added to the CodeIgniter package are directories intended to be unique to each student. In an unused portfolio, especially a source from which copies would be made, these directories would exist as empty folders.</p>
<ul>
<li>docs -- the resumes go here.</li>
<li>imgs -- the screenshots go here.(In the event that we further enhance the project to allow the public to click on the screenshots and see a larger image, I would recommend that the original, unresized images either go in this directory under similar names with a common prefix or in a separate directory within "cms"; jQuery-style gallery options may be explored as well.)</li><br />
</ul><br />
<strong><em>Goals for Deployment</em></strong></p>
<p>[Documentation in Progress]</p>
<p>Presumably a Bash script run on the Apache server would read ┬&aacute;from a list of student LDAP usernames (and related information?), create directories named after the usernames, and insert files and folders from an unused portfolio.</p>
<p>If the temporary database-based login system currently in use during development is retained, the script must also add student records to the "users" table. A purely LDAP-based login system would eliminate this step.</p>
<p><em><strong>Remaining Issues and Enhancements</strong></em></p>
<p>Lest we add any enhancements or correct issues for each of many student portfolios, the following issues and enhancements should be addressed before deployment so that we have a relatively ideal if not perfect portfolio from which to make copies.</p>
<ul>
<li>Part of the original goals for the portfolio project include a jQuery Mobile version of the public portfolio pages. Code is already in place to determine whether a mobile device is calling the portfolio and, if so, redirect to another page which would display a mobile version. Said mobile version has yet to be built.</li>
<li>A version of the student portfolio is currently serving as a live "guinea pig", linked on the original developer's own LinkedIn profile for all LinkedIn members to see. Via LinkedIn, the developer has received at least one suggestion, particularly the option of viewers "zooming" onto larger versions of the screenshots.</li>
<li>Conversion from a database-based login sytem to an LDAP-based login system is in progress.</li><br />
</ul></p>
