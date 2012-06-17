---
layout: post
title: "How to build website using Github Pages and Jekyll?"
description: ""
category: 
tags: []
---
{% include JB/setup %}

<br><br>
### Create new repository
In the github website, under your account create a new repository name `duraiselvaa.github.com`. Substitute `duraiselvaa` to your `account name`

<br><br>
### Tutorial for jekyll boostrap

Formal doc can be found at [jekyllbootstrap](http://jekyllbootstrap.com/) but below is mine on how to set up website for duraiselvaa. If you have any issues/queries please visit jekyllboostrap.com for broader knowledge.

<br><br>
### Clone jekyllbootstrap locally & push

as specified in the `jekyllbootstrap` website follow the below instructions to clone


	$ git clone https://github.com/plusjade/jekyll-bootstrap.git duraiselvaa.github.com
	$ cd duraiselvaa.github.com
	$ git remote set-url origin git@github.com:duraiselvaa/duraiselvaa.github.com.git
	$ git push origin master


At this point, when our changes are finally pushed, Github will monitor the changes & try to build the contents under jekyll site generator. You will receive a email from github if the build is success/failure. In sometime (about 10 mins) you will find the contents in action at http://duraiselvaa.github.com

<br><br>
### Jekyll set up - for local testing (windows platform)

As we are first planning to test and run the website locally we need to install `Ruby & Dev Kit` and install the gem `jekyll` in Ruby.

1. visit http://rubyinstaller.org/downloads/ and download & install [Ruby 1.9.3-p194](http://rubyforge.org/frs/download.php/76054/rubyinstaller-1.9.3-p194.exe)

2. Then download and extract [DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe](https://github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe). Lets call the extracted folder DEVKIT_INSTALL_DIR (C:/Programs/RubyDevKit)

3. Add `DEVKIT_INSTALL_DIR/bin` path to PATH env variable

4. Run the below commands to setup.

commands:

	$ cd DEVKIT_INSTALL_DIR
	$ ruby dk.rb init
	$ ruby dk.rb review
	$ ruby dk.rb install
	$ gem install rdiscount --platform=ruby // to test the installation
	$ gem install jekyll                    // finally ... our gem of interest

<br><br>
### Running our website repo with jekyll

	cd duraiselvaa.github.com
	jekyll --server


At this point jekyll should be successfully monitoring the contents under `duraiselvaa.github.com` and generating website pages dynamically, which you can see it in action at [http://localhost:4000](http://localhost:4000)

<br><br>
### First cleanup

As & when you do any of below changes, you can find the website at [http://localhost:4000](http://localhost:4000) constantly refreshed by jekyll


<br>
##### a. update _config.yml

Update this file to update details on `title, tagline, author`

<br>
##### b. delete unnecessary files

Run below commands (unix-like) to delete unwanted files in duraiselvaa.github.com folder (you could also manually delete the files using explorer)


	rm -rf _posts/core-samples
	rm tags.html
	rm pages.html
	rm categories.html

<br>
##### c. update index.md

delete all the contents of this file and replace with 

	---
	layout: page
	---

	% include JB/setup %}

<br>
##### d. rename 'Archive' to 'Blog' (my preference)

You can rename `Archive.html` to `blog.html` and edit its file contents to change the `title = Blog`

Also edit `_includes/themes/twitter/post.html` to change the label in pagination control from `Archive -> Blog List`

Then edit `_config.yml` to change `archive_path: /archive.html` to `archive_path: /blog.html`

<br>
##### e. create Test blogs

For creating posts, use `rake task` command


	rake post title="blog1" date="01/01/2012"
	rake post title="blog2"                     // current date is used as default


You don't have to put contents into blog post now. We could do it github.com in the way we use to edit wiki pages but with Maruku markdown. 

<br><br>
### Lets 'push' and blog

Use git to push all our cleanup changes. Github should publish the new changes in [http://duraiselvaa.github.com](http://duraiselvaa.github.com)

Now navigate to github folder where our blog is created [https://github.com/duraiselvaa/duraiselvaa.github.com/tree/master/_posts](https://github.com/duraiselvaa/duraiselvaa.github.com/tree/master/_posts)

Click on the blog__.md file and then click on `Edit this file` to open the file in edit mode. Add your blog contents here and click `Commit Changes`

On commit github should publish the blog.


<br><br>
### Blogging using 'Maruku markdown'

[Maruku](http://maruku.rubyforge.org/maruku.html) is the default markdown engine of Ruby and is default of Github's Jekyll too.
Remember [Gollom](https://github.com/github/gollum) is the default markdown of github wiki. So don't mixup.


<br><br>
### Configuring Disqus & Google Analytics


Register at [Disqus](http://disqus.com/admin/register/) and know your `shortname`. I have updated the Moderators settings so that all the comments are approved first before being published.

Also Signup/Login into [Google Analytics](http://www.google.com/analytics/) and find your `Tracking ID`

Update both the ids into `_config.yml`

	...
	  comments :
	    provider : disqus
	    disqus :
	      short_name : selvaduraisamy
	...
	
	  analytics :
	    provider : google 
	    google : 
	        tracking_id : 'UA-3266xxxx-1'
	...
	
	
Comment out unused services.

<br><br>
### Adding more pages to duraiselvaa

use below basic code to create pages (manually or using ```rake page name="page1.md" ``` )


	---
	layout: page
	title: Demo
	group: navigation
	weight : 5
	---
	
	% include JB/setup %}


Define the `weight` attribute in all the necessary pages.


<br><br>
### Sorting the page links in the Nav Panel

By default jekyll bootstrap code doesn't support weight to the pages. In order to support weight update `_includes\JB\pages_list` (find my update on '+' lines)


	+  % for weight in (1..10) %}
	   % for node in pages_list %}
	+  % if node.weight == weight %}
	     % if node.title != null %}
	       % if group == null or group == node.group %}
	       	% if page.url == node.url %}
	       	<li class="active"><a href="{{ BASE_PATH }}{{node.url}}" class="active">{{node.title}}</a></li>
	       	% else %}
	       	<li><a href="{{ BASE_PATH }}{{node.url}}">{{node.title}}</a></li>
	       	% endif %}
	       % endif %}
	     % endif %}
	+  % endif %}
	   % endfor %}
	+  % endfor %}



<br><br>
### Cool Stuff - with twitter bootstrap

Since twitter-bootstrap is the theme used, we can use its [Base css](http://twitter.github.com/bootstrap/base-css.html) and [Components](http://twitter.github.com/bootstrap/components.html) directly in the pages.md


<br><br>
### Finally www.duraisamy.co.uk

All this time our changes were published to [http://duraiselvaa.github.com](http://duraiselvaa.github.com)

In order to use our own domain name, loginto your domain registrar admin panel & create a `CNAME` or `A` entry 

I created below two `A` records to handle/redirect [http://duraisamy.co.uk](http://duraisamy.co.uk) and [http://www.duraisamy.co.uk](http://www.duraisamy.co.uk) to github's 204.232.175.78 


	DNS Entry = @; Type = A; Target=204.232.175.78
	DNS Entry = www; Type = A; Target=204.232.175.78


Otherwise create a `CNAME` record as follows


	DNS Entry = www; Type = CNAME; Target=duraiselvaa.github.com


Then add a file named `CNAME` (no .extension for this) to duraiselvaa.github.com repo with content
	
	www.duraisamy.co.uk

<br>