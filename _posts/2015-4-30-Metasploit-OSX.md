---
layout: post
title: Metasploit on OSX
---
[labrat's guide on installing metasploit on maverick](http://www.labrat.com/2014/02/how-to-install-metasploit-on-mavericks.html)

used this guide to install metasploit on osx yosemite. It worked great however when install the pg gem there was no way to manually install this with the proper hooks. Running msfupdate to build the gems fixed it.

Notes on limitations however:

you can't just include the MSF path to run all the metasploit tools from anywhere as the metasploit framework gem seems to only exist when running the tools from the directory. This seems weird but the metasploit framework gem doesn't actually exist in the MSF gemset anyway.

Backgrounding sessions in msfconsole throws internal framework errors. However, it works perfectly so go figure.

The database does not automatically connect so on starting msfconsole you have to manually connect it to the postgresql database using:


{% highlight ruby %}
 db_connect msf:msf@127.0.0.1/msf
{% endhighlight %}


so just add this to the startup resource file 
