# SPAN-ipedia Wiki

## Contents
  1. [Introduction](#introduction)
  2. [Logging In / Creating a login](#logging-in)
      1. [Create a Login](#create-a-login)
      2. [Keeping Updated](#keeping-updated)
  3. [Help & Reference](#help-and-reference)
      1. [Quick Wiki Tips](#quick-wiki-tips)
      2. [MediaWiki Help](#mediawiki-help)
  4. [Editing Navigation Side Bar](#editing-navigation-side-bar)
  5. [Uploading Files](#uploading-files)
      1. [Security](#security)
  6. [Website Tracking / Google Analytics](#website-tracking)
  7. [Wiki Backup](#wiki-backup)
      1. [Cleaning up Old Wiki Backups](#cleaning-up-old-wiki-backups)
      2. [Restoring from a wiki backup](#restoring-from-a-wiki-backup)
      
<a name='introduction'></a>
## Introduction
SPAN-ipedia is a wiki reference for the SPAN Lab which uses the ubiquitous MediaWiki software. The same PHP, MySQL based software used for wikipedia. Wiki's are easy to update, which makes it ideal for documents and resources which are often updated.

SPAN-ipedia includes the lab manual and other resources for the lab.

Feel free to contribute, add, and update. The more people contribute. The more useful the wiki becomes.

<a name='logging-in'></a>
## Logging In / Creating a login
You will have to login to view this wiki. Login as 'span' or 'admin'. See lab manager for password. All pages are set to be private. These settings can be found in in the LocalSettings.php configuration file.

You can set a particular page to be viewable to the public by editing the following line in LocalSettings.php
```
e.g.  $wgWhitelistRead = array ("Main_Page","Lab_Manual");
```
You can either login with the 'span' username, or you can login as 'span' and create a username for yourself. You can find the user management tools in the menu under toolbox -> special pages.

If you create your own login, you can login with that name and people will be able to see which edits are made by you. You can also get access to cool features such as getting email updates when a specific page is changed by using the 'watch' feature!

<a name='create-a-login'></a>
### Create a Login
First Create Your Login:
  1. Login as span
  2. Go to toolbox > special pages > login / create account
  3. Click on __Create Account__
  4. Create your Account
  
Now Edit Your User Rights:
  1. Login as span
  2. Go to toolbox > special pages > restricted special pages > user rights management
  3. Add yourself to the sysop / bureaucrats groups
  
<a name='keeping-updated'></a>
### Keeping Updated
If you have created your own SPAN-ipedia account, you can __watch__ a page, by clicking on the 'watch' button at the top of the page.

This will keep you updated when a page is updated.

<a name='help-and-reference'></a>
## Help & Reference

<a name='quick-wiki-tips'></a>
### Quick Wiki Tips
  - Login using the "login" link in the upper right.
  - Browse SPAN-ipedia using the navigation menu on the left.
  - Search SPAN-ipedia using the search box.
  - Edit a page or a section by clicking on edit at the top of a page or next to a section.
  - Create a new page by creating a link in an existing page, or searching for the page title you want, and clicking on creat this page.
  - For Help with MediaWiki syntax for formatting, visit: http://meta.wikimedia.org/wiki/Help:Editing
  - [wikipedia reference card (pdf)](https://web.stanford.edu/group/spanlab/cgi-bin/wiki/images/MediaWikiRefCard.pdf)
  - [MediaWiki Editing Examples - http://meta.wikimedia.org/wiki/Help:Wikitext_examples](http://meta.wikimedia.org/wiki/Help:Wikitext_examples)
  
<a name='mediawiki-help'></a>
### MediaWiki Help
Consult the [User's Guide](http://meta.wikimedia.org/wiki/Help:Contents) for information on using the wiki software.
  - [Configuration settings list](http://www.mediawiki.org/wiki/Manual:Configuration_settings)
  - [MediaWiki FAQ](http://www.mediawiki.org/wiki/Manual:FAQ)
  
<a name='editing-navigation-side-bar'></a>
## Editing Navigation Side Bar
You can edit the Navigation Side Bar at http://spanlab.stanford.edu/wiki/index.php/MediaWiki:Sidebar

<a name='uploading-files'></a>
## Uploading Files
Only certain file types are uploadable. These can be changed and are specified in the wiki/includes/DefaultSettings.php file.
```
 e.g. $wgFileExtensions = array( 'png', 'gif', 'jpg', 'jpeg', 'pdf', 'mov' );
```

<a name='security'></a>
### Security
The standard MediaWiki files are uploaded in an insecure fashion. Anyone can access them by going to an URL (e.g. http://spanlab.stanford.edu/wiki/images/example.doc)

To secure files, three changes need to be made.

In .htaccess file in images directory
```
 Deny from All
```
In [path/to/wiki]/includes/DefaultSettings.php
```
 $wgUploadPath       = "{$wgScriptPath}/img_auth.php";
```
In /etc/apache2/httpd.conf add lines:
```
 Alias /home/span/public_html/spanlab.stanford.edu/wiki/images/ /home/span/public_html/spanlab.stanford.edu/wiki/img_auth.php
 Alias /home/span/public_html/spanlab.stanford.edu/wiki/images /home/span/public_html/spanlab.stanford.edu/wiki/img_auth.php
```

<a name='website-tracking'></a>
## Website Tracking / Google Analytics
Tracking code was added using Google Analytics as javascript to the bottom of the Monobook.php file under ./wiki/skins/MonoBook.php, and can be accessed with the _spanlab_ google account at http://www.google.com/analytics

<a name='wiki-backup'></a>
## Wiki Backup
Wiki backup has been implemented using a script in /biac2/data2/wiki_backup/wikibackup_script

It can be run manually using the command
```
 /biac2/data2/wiki_backup/wikibackup_script
```
The script is enabled to run every night through a crontab on amygdala.

You can edit the crontab using
```
 crontab -e
```
There are three types of wiki backups created.
  1. One is a backup of the files in the /home/span/public_html/spanlab.stanford.edu/wiki directory.
  2. A second file is a backup of the MySQL database the wiki lives on
  3. A third file is an XML backup of the media wiki page. This XML backup can be used to import a wiki or to export to a different wiki software.

<a name='cleaning-up-wiki-backups'></a>
### Cleaning up Old Wiki Backups
The backups are timestamped. Old backups need to be cleaned out every once in a while for space.

A script has been added to delete old wiki backups at /biac2/data2/wiki_backup/wikibackup_cleanup_script

It can be run manually using the command
```
 /biac2/data2/wiki_backup/wikibackup_cleanup_script
```
The script is enabled to run every night through a crontab on amygdala.

You can edit the crontab using
```
 crontab -e
```

<a name='restoring-from-a-wiki-backup'></a>
### Restoring from a wiki backup
restore sql:
```
 gunzip -c wiki-timestamp.sql.gz | mysql dbname>
```
restore xml:
```
 php maintenance/importDump.php wiki-timestamp.xml.gz
```
for more info on importing XML dumps, see http://www.mediawiki.org/wiki/Manual:Importing_XML_dumps
