---
layout: post
status: publish
published: true
title: 'ITM MassEmail: How to Use'
author:
  display_name: Jeffrey Hartmann
  login: jhartma3
  email: jhartma3@hawk.iit.edu
  url: http://www.linkedin.com/in/jeffreyhartmann
author_login: jhartma3
author_email: jhartma3@hawk.iit.edu
author_url: http://www.linkedin.com/in/jeffreyhartmann
date: '2012-07-15 11:54:51 -0500'
date_gmt: '2012-07-15 17:54:51 -0500'
tags: []
comments: []
---
<p>MassEmail is a project designed for the Information Technology and Management Department (ITM) of the Illinois Institute of Technology (IIT) to ease the sending of mass e-mail announcements to department students, whether undergraduate, graduate or both.</p>
<p>The website can be found at http://diamond.sat.iit.edu/MassEmail/. As it is a PHP-based website, the URL is case-sensitive.</p>
<p><strong>I. The MassEmail Database</strong></p>
<p>For those who hold the proper credentials, the database can be found at http://diamond.sat.iit.edu/phpmyadmin/. Upon successfully logging in, look for a database called &Gamma;&Ccedil;&pound;<strong>massemail</strong>&Gamma;&Ccedil;&yen; (all lower-case).</p>
<p>The &Gamma;&Ccedil;&pound;massemail&Gamma;&Ccedil;&yen; database consists of three tables:</p>
<ul>
<li>&Gamma;&Ccedil;&pound;<strong>useradmins</strong>&Gamma;&Ccedil;&yen; stores the users who are permitted to use the site. Maintenance of the users is handled directly onto this table.
<ul>
<li>&Gamma;&Ccedil;&pound;<strong>username</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the LDAP username.</li>
<li>&Gamma;&Ccedil;&pound;<strong>namefirst</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the user&Gamma;&Ccedil;&Ouml;s first name.</li>
<li>&Gamma;&Ccedil;&pound;<strong>namelast</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the user&Gamma;&Ccedil;&Ouml;s last name.</li>
<li>&Gamma;&Ccedil;&pound;<strong>email_first</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the user&Gamma;&Ccedil;&Ouml;s primary or preferred IIT e-mail address. Users may use only IIT-related e-mail addresses to send e-mails via the MassEmail website.</li>
<li>The remaining fields (&Gamma;&Ccedil;&pound;<strong>id_useradmin</strong>&Gamma;&Ccedil;&yen;, &Gamma;&Ccedil;&pound;<strong>datecreated</strong>&Gamma;&Ccedil;&yen;, &Gamma;&Ccedil;&pound;<strong>dateupdated</strong>&Gamma;&Ccedil;&yen;) are populated automatically.</li><br />
</ul><br />
</li></p>
<li>&Gamma;&Ccedil;&pound;<strong>students</strong>&Gamma;&Ccedil;&yen; stores the outgoing mailing list, consisting of IIT ITM students. We update this table via the MassEmail website by uploading data from a CSV file.
<ul>
<li>&Gamma;&Ccedil;&pound;<strong>email</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the student&Gamma;&Ccedil;&Ouml;s email address.</li>
<li>&Gamma;&Ccedil;&pound;<strong>name</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; the student&Gamma;&Ccedil;&Ouml;s full name.</li>
<li>&Gamma;&Ccedil;&pound;<strong>status</strong>&Gamma;&Ccedil;&yen; &Gamma;&Ccedil;&ocirc; this field determines whether the student is either &Gamma;&Ccedil;&pound;Graduate&Gamma;&Ccedil;&yen; or &Gamma;&Ccedil;&pound;Undergraduate&Gamma;&Ccedil;&yen;.</li>
<li>The remaining field (&Gamma;&Ccedil;&pound;<strong>id_students</strong>&Gamma;&Ccedil;&yen;) is populated automatically.</li><br />
</ul><br />
</li></p>
<li>&Gamma;&Ccedil;&pound;<strong>test_students</strong>&Gamma;&Ccedil;&yen; is an alternate, &Gamma;&Ccedil;&pound;test&Gamma;&Ccedil;&yen; version of the &Gamma;&Ccedil;&pound;students&Gamma;&Ccedil;&yen; table, with the same exact field structure, consisting of &Gamma;&Ccedil;&pound;fake&Gamma;&Ccedil;&yen; data. This is used for trouble-shooting. (An alternate version of this project, &Gamma;&Ccedil;&pound;MassEmailTest&Gamma;&Ccedil;&yen;, may be used for this purpose.)</li><br />
</ul><br />
<strong>II. The MassEmail Login</strong></p>
<p>Users simply login at MassEmail with the same username and password which they use at &Gamma;&Ccedil;&pound;my.iit.edu" and other IIT web portals.</p>
<p>The Mass email first checks the username against the database; then it checks both username and password against the LDAP system. (We store only usernames and not passwords in the &Gamma;&Ccedil;&pound;massemail&Gamma;&Ccedil;&yen; database.)</p>
<p>Upon a successful login, the user is greeted with the following options:</p>
<ul>
<li>Upload Student Mailing List</li>
<li>Send a Mass E-Mail to Students</li>
<li>Logout</li><br />
</ul><br />
These options also appear throughout the site in the navigation section.</p>
<p>Throughout the website, you may see a note indicating the last time the &Gamma;&Ccedil;&pound;students&Gamma;&Ccedil;&yen; table was updated (based on information from the live database). Because the user might first want to determine whether the mailing list data is fresh and up to date, we shall next discuss &Gamma;&Ccedil;&pound;Upload Student Mailing List&Gamma;&Ccedil;&yen;.</p>
<p><strong>III. Upload Student E-Mailing List</strong></p>
<p>The mailing list is based on a query run off the Cognos system, which should generate a CSV file consisting of the following data fields in the following order:</p>
<ol>
<li>Student e-mail address</li>
<li>Student name</li>
<li>Student status (Graduate or Undergraduate)</li><br />
</ol><br />
You may name the CSV file anything you wish, as long as you are able to locate it easily within your system. Upon downloading the CSV file to your computer, you would then upload the data from this file to the &Gamma;&Ccedil;&pound;massemail&Gamma;&Ccedil;&yen; database&Gamma;&Ccedil;&Ouml;s &Gamma;&Ccedil;&pound;students&Gamma;&Ccedil;&yen; table via the &Gamma;&Ccedil;&pound;Upload&Gamma;&Ccedil;&ordf;&Gamma;&Ccedil;&yen; page in the following steps:</p>
<ol>
<li>Browse for your CSV file.</li>
<li>Click &Gamma;&Ccedil;&pound;Upload Mailing List&Gamma;&Ccedil;&yen;.</li><br />
</ol><br />
Behind the scenes, the website instructs the database to truncate (delete) all the old data from the &Gamma;&Ccedil;&pound;students&Gamma;&Ccedil;&yen; table and add new records to the table, one at a time, automatically skipping over any CSV record with a blank e-mail address.</p>
<p>Upon a successful update, an alert should appear stating that the data was updated. Click &Gamma;&Ccedil;&pound;OK&Gamma;&Ccedil;&yen; and you return to the &Gamma;&Ccedil;&pound;Updating&Gamma;&Ccedil;&ordf;&Gamma;&Ccedil;&yen; page, where you should notice that the time and date has changed into the current one for when the mailing list was last updated.</p>
<p><strong>IV. Sending a MassEmail</strong></p>
<p>We take you through a simple four-stage process for sending a mass e-mail to your students.</p>
<ol>
<li><em><strong>&Gamma;&Ccedil;&pound;FROM&Gamma;&Ccedil;&yen;</strong></em><br />
You have the option of sending your e-mail under your own name and e-mail address or under a generic department-level e-mail address [currently to be determined]. Make your choice and click &Gamma;&Ccedil;&pound;Go to Step 2 of 3&Gamma;&Ccedil;&yen;.</li></p>
<li><em><strong>&Gamma;&Ccedil;&pound;TO&Gamma;&Ccedil;&yen;</strong></em><br />
You have the option of sending your e-mail to either one of the following:</p>
<ul>
<li>All Students</li>
<li>Undergraduate Students</li>
<li>Graduate Students</li><br />
</ul><br />
Simply make your choice.</p>
<p>If necessary you may copy your e-mail to others. If so, in the textbox enter at least one e-mail address (no names). If more than one e-mail address, please use only commas to separate them. Otherwise, please leave this box empty.</p>
<p>All &Gamma;&Ccedil;&pound;TO&Gamma;&Ccedil;&yen; e-mail addresses, whether students or copies, appear in the BCC (Blind Carbon Copy) queue. Each recipient would see the phrase &Gamma;&Ccedil;&pound;undisclosed recipients&Gamma;&Ccedil;&yen; in the actual &Gamma;&Ccedil;&pound;TO&Gamma;&Ccedil;&yen; queue.</p>
<p>When you are satisfied with your choices, click &Gamma;&Ccedil;&pound;Go to Step 3 of 3&Gamma;&Ccedil;&yen;.</li></p>
<li><em><strong>&Gamma;&Ccedil;&pound;COMPOSE&Gamma;&Ccedil;&yen;</strong></em><br />
Here is where you compose and send your mass e-mail.</p>
<ul>
<li>&Gamma;&Ccedil;&pound;Subject&Gamma;&Ccedil;&yen; is required.</li>
<li>&Gamma;&Ccedil;&pound;Message&Gamma;&Ccedil;&yen; (the body of the e-mail) is required. Here you have the option of formatting whatever font you may choose within the body.</li>
<li>&Gamma;&Ccedil;&pound;Attachment&Gamma;&Ccedil;&yen; is optional. Only one attachment is allowed for each mass e-mail. Otherwise, leave this option blank.</li><br />
</ul><br />
When you are satisfied with your e-mail, click &Gamma;&Ccedil;&pound;SEND EMAILS&Gamma;&Ccedil;&yen;.</li></p>
<li><em><strong>The Alert Message</strong></em><br />
An alert should appear, telling you how many student emails were sent, how many copies if any were sent, and whether or not an attachment was sent as well. Upon clicking &Gamma;&Ccedil;&pound;OK&Gamma;&Ccedil;&yen;, you return to the Step One page of the e-mailing process.</li><br />
</ol><br />
<strong>V. Logout</strong></p>
<p>At any point during your visit, you may simply click &Gamma;&Ccedil;&pound;Logout&Gamma;&Ccedil;&yen; in the navigation panel, and you are done.</p>
<p><strong>VI. Developer Hints (Troubleshooting and Configuration)</strong></p>
<p><em><strong>About This Site</strong></em></p>
<p>MassEmail was developed using straight-forward, procedural PHP. A MySQL database drives this project.</p>
<p>Although MassEmail is not a necessarily an MVC project or uses any special frameworks, the developer adapted a few basic file management principles from such projects. For example, much of the HTML development is found in the &Gamma;&Ccedil;&pound;views&Gamma;&Ccedil;&yen; directory.</p>
<p><em><strong>Config Files</strong></em></p>
<p>The developer created a &Gamma;&Ccedil;&pound;config&Gamma;&Ccedil;&yen; directory to store some general configuration variables and processes applied and reused throughout the website. Often the developer used the files within &Gamma;&Ccedil;&pound;config&Gamma;&Ccedil;&yen; to toggle between live and test versions of the project. Many of these files are &Gamma;&Ccedil;&pound;included&Gamma;&Ccedil;&yen; at the top of the web pages -- often in a certain sequence as one file may depend on variables populated in another file. In the event that his project and its database may be relocated to another server, appropriate changes may be made to these files.</p>
<p><em><strong>The E-Mail Body: Courtesy of TinyMCE</strong></em></p>
<p>The &Gamma;&Ccedil;&pound;WYSIWYG&Gamma;&Ccedil;&yen; interface currently in use for the body portion of the outgoing e-mails is based on an editor called TinyMCE, developed by MoxieCode Systems AB. For updates and other issues regarding TinyMCE, we suggest that you visit http://www.tinymce.com/.</p>
<p><em><strong>AJAX Login</strong></em></p>
<p>The login system (ajax_login.php and related files) is based on material from http://roshanbh.com.np.</p>
