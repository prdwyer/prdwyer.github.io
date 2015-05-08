---
layout: post
title: Nmap Scripting Engine 
---

Nmap typically is used as a networking tool to find open ports. It has quite the suite of capabilities installed with it that give a incredible amount of reconissance power and push its pentest lifecycle use out of the reconissance phase into vulnerability scanning and detection. 

Some of nmaps more mature and automated abilites come from the scripting engine these are invoked via the option --script. There are two types of files being used by nse.Nse library files have a .lua extension and reside in /share/nmap/nselib/. The actual scripts live in /share/nmap/scripts/. 

nmap can update its own script database:
{% highlight bash %}
➜  ~  nmap --script-updatedb 
{% endhighlight %}

One of the most common nse scripts is the banner script. This just grabs the banners out of any applicable services during a scan. 
{% highlight bash %}
➜  ~  nmap --script banner 192.168.5.117

Starting Nmap 6.47 ( http://nmap.org ) at 2015-05-08 17:28 EDT
Nmap scan report for owaspbwa.lan (192.168.5.117)
Host is up (0.048s latency).
Not shown: 991 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
|_banner: SSH-2.0-OpenSSH_5.3p1 Debian-3ubuntu4
80/tcp   open  http
139/tcp  open  netbios-ssn
143/tcp  open  imap
| banner: * OK [CAPABILITY IMAP4rev1 UIDPLUS CHILDREN NAMESPACE THREAD=OR
|_DEREDSUBJECT THREAD=REFERENCES SORT QUOTA IDLE ACL ACL2=UNION] Couri...
443/tcp  open  https
445/tcp  open  microsoft-ds
5001/tcp open  commplex-link
|_banner: \xAC\xED\x00\x05
8080/tcp open  http-proxy
8081/tcp open  blackice-icecap

Nmap done: 1 IP address (1 host up) scanned in 13.93 seconds
{% endhighlight %}

scripts can be run individually using their script name or many of of them can be run by categories. 

*auth - Scripts that can take user name and password to obtain information from services. Also includes scripts that attempt to enumerate users.

*broadcast - Mostly discovery scripts that attempt to enumerate hosts based upon various types of broadcast packets. 

*brute - Brute forcing various services.

*default - These scripts are the ones run by the -sC option

*discovery - Querying the maximum information out of services 

*dos - Scripts that test for and perform a few types of dos attacks. There aren't a ton of them but there are slowloris, avahi, and smb dos scripts.

*exploit - These scripts detect various CVEs  and vulnerabilities on the target.

*vuln - Scans for vulnerabilities. This category is very cool. Its great to point nmap at at web server and instantly be pulling out sqli vulnerabilites rather than spinning up a intensive vulnerability scanner. 

*safe - low impact quiet scripts. These are stealthier.



Note that many scripts overlap categories. For intance many of the scripts run with the exploit category will be run using the vuln.

