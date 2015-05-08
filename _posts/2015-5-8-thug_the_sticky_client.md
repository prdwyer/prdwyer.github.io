---
layout: post
title: Tutorial on using Thug.py : a sweet sticky client
---

Most "honey" software is intended to emulate a server side to get intelligence from accessing attempts. Client side attacks are becoming common and there to research these attacks there is a need for emulating a client. This is what thug does. It requests a target url, follows redirects and obtains all javascript and shellcode from potentially malicious urls. 

Thug can be found on the excellent honeydrive linux distribution but the project is being very actively maintained and newer version can be cloned right from [https://github.com/buffer/thug](https://github.com/buffer/thug)

Thug has a lot of capabilites as you can see from the a -h

{% highlight tcsh %}

 Thug: Pure Python honeyclient implementation

    Usage:
        python thug.py [ options ] url

    Options:
        -h, --help          	Display this help information
        -V, --version       	Display Thug version
        -u, --useragent=    	Select a user agent (see below for values, default: winxpie60)
        -e, --events=       	Enable comma-separated specified DOM events handling
        -w, --delay=        	Set a maximum setTimeout/setInterval delay value (in milliseconds)
        -n, --logdir=       	Set the log output directory
        -o, --output=       	Log to a specified file
        -r, --referer=      	Specify a referer
        -p, --proxy=        	Specify a proxy (see below for format and supported schemes)
        -l, --local         	Analyze a locally saved page
        -x, --local-nofetch 	Analyze a locally saved page and prevent remote content fetching
        -v, --verbose       	Enable verbose mode
        -d, --debug         	Enable debug mode
        -q, --quiet         	Disable console logging
        -m, --no-cache      	Disable local web cache
        -a, --ast-debug     	Enable AST debug mode (requires debug mode)
        -g, --http-debug    	Enable HTTP debug mode 
        -t, --threshold     	Maximum pages to fetch
        -E, --extensive     	Extensive fetch of linked pages
        -T, --timeout=      	Set the analysis timeout (in seconds)
        -B, --broken-url    	Set the broken URL mode
        -y, --vtquery       	Query VirusTotal for samples analysis
        -s, --vtsubmit      	Submit samples to VirusTotal
        -N, --no-honeyagent 	Disable HoneyAgent support

        Plugins:
        -A, --adobepdf=     	Specify the Adobe Acrobat Reader version (default: 9.1.0)
        -P, --no-adobepdf   	Disable Adobe Acrobat Reader plugin
        -S, --shockwave=    	Specify the Shockwave Flash version (default: 10.0.64.0)
        -R, --no-shockwave  	Disable Shockwave Flash plugin
        -J, --javaplugin=   	Specify the JavaPlugin version (default: 1.6.0.32)
        -K, --no-javaplugin 	Disable Java plugin

        Classifier:
        -Q, --urlclassifier 	Specify a list of additional (comma separated) URL classifier rule files
        -W, --jsclassifier  	Specify a list of additional (comma separated) JS classifier rule files
        -C, --sampleclassifier 	Specify a list of additional (comma separated) sample classifier rule files

    Proxy Format:
        scheme://[username:password@]host:port (supported schemes: http, http2, socks4, socks5)

    Available User-Agents:
	winxpie60		Internet Explorer 6.0	(Windows XP)
	winxpie61		Internet Explorer 6.1	(Windows XP)
	winxpie70		Internet Explorer 7.0	(Windows XP)
	winxpie80		Internet Explorer 8.0	(Windows XP)
	winxpchrome20		Chrome 20.0.1132.47	(Windows XP)
	winxpfirefox12		Firefox 12.0		(Windows XP)
	winxpsafari5		Safari 5.1.7		(Windows XP)
	win2kie60		Internet Explorer 6.0	(Windows 2000)
	win2kie80		Internet Explorer 8.0	(Windows 2000)
	win7ie80		Internet Explorer 8.0	(Windows 7)
	win7ie90		Internet Explorer 9.0	(Windows 7)
	win7chrome20		Chrome 20.0.1132.47	(Windows 7)
	win7firefox3		Firefox 3.6.13		(Windows 7)
	win7safari5		Safari 5.1.7		(Windows 7)
	osx10safari5		Safari 5.1.1		(MacOS X 10.7.2)
	osx10chrome19		Chrome 19.0.1084.54	(MacOS X 10.7.4)	linuxchrome26		Chrome 26.0.1410.19	(Linux)
	linuxchrome30		Chrome 30.0.1599.15	(Linux)
	linuxfirefox19		Firefox 19.0		(Linux)
	galaxy2chrome18		Chrome 18.0.1025.166	(Samsung Galaxy S II, Android 4.0.3)
	galaxy2chrome25		Chrome 25.0.1364.123	(Samsung Galaxy S II, Android 4.0.3)
	galaxy2chrome29		Chrome 29.0.1547.59	(Samsung Galaxy S II, Android 4.1.2)
	nexuschrome18		Chrome 18.0.1025.133	(Google Nexus, Android 4.0.4)
	ipadsafari7		Safari 7.0		(iPad, iOS 7.0.4)
	ipadchrome33		Chrome 33.0.1750.21	(iPad, iOS 7.1)
	ipadchrome35		Chrome 35.0.1916.41	(iPad, iOS 7.1.1)

{% endhighlight %}


It supports all kinds of user agents, allows you to send the samples directly to virustotal and gives you the option to specify different plugins. The proxy support is also a great feature.

To demo it I threw up a standard metasploit browser autopwn module and pointed thug at it without using any options.
{% highlight tcsh %}
sudo python thug.py http://192.168.5.105:8080/test

[2015-05-08 00:42:32] [window open redirection] about:blank -> http://192.168.5.105:8080/test/
[2015-05-08 00:42:32] [HTTP] URL: http://192.168.5.105:8080/test/ (Status: 404, Referrer: None)
[2015-05-08 00:42:32] [File Not Found] URL: http://192.168.5.105:8080/test/
[2015-05-08 00:42:32] Saving log analysis at ../logs/bf562b75e826f814c31d649ec22990ea/20150508004231
honeydrive@honeydrive:/honeydrive/thug/src$ sudo python thug.py http://192.168.5.105:8080/test
[2015-05-08 00:42:42] [window open redirection] about:blank -> http://192.168.5.105:8080/test
[2015-05-08 00:42:43] [HTTP] URL: http://192.168.5.105:8080/test (Status: 200, Referrer: None)
[2015-05-08 00:42:43] [HTTP] URL: http://192.168.5.105:8080/test (Content-type: text/html, MD5: c0e703784116c3367d9454be61879d2b)
[2015-05-08 00:42:44] ActiveXObject: microsoft.xmlhttp
[2015-05-08 00:42:44] [Microsoft XMLHTTP ActiveX] open('GET', 'http://192.168.5.105:8080/test?sessid=V2luZG93cyBYUDp1bmRlZmluZWQ6dW5kZWZpbmVkOnVuZGVmaW5lZDp1bmRlZmluZWQ6ZW46eDg2OnVuZGVmaW5lZDp1bmRlZmluZWQ6', True)
[2015-05-08 00:42:44] [Microsoft XMLHTTP ActiveX] send
[2015-05-08 00:42:44] [Microsoft XMLHTTP ActiveX] Fetching from URL http://192.168.5.105:8080/test?sessid=V2luZG93cyBYUDp1bmRlZmluZWQ6dW5kZWZpbmVkOnVuZGVmaW5lZDp1bmRlZmluZWQ6ZW46eDg2OnVuZGVmaW5lZDp1bmRlZmluZWQ6 (method: GET)
[2015-05-08 00:42:44] [Microsoft XMLHTTP Exploit redirection] http://192.168.5.105:8080/test -> http://192.168.5.105:8080/test?sessid=V2luZG93cyBYUDp1bmRlZmluZWQ6dW5kZWZpbmVkOnVuZGVmaW5lZDp1bmRlZmluZWQ6ZW46eDg2OnVuZGVmaW5lZDp1bmRlZmluZWQ6
[2015-05-08 00:42:47] [HTTP] URL: http://192.168.5.105:8080/test?sessid=V2luZG93cyBYUDp1bmRlZmluZWQ6dW5kZWZpbmVkOnVuZGVmaW5lZDp1bmRlZmluZWQ6ZW46eDg2OnVuZGVmaW5lZDp1bmRlZmluZWQ6 (Status: 200, Referrer: http://192.168.5.105:8080/test)
[2015-05-08 00:42:47] [HTTP] URL: http://192.168.5.105:8080/test?sessid=V2luZG93cyBYUDp1bmRlZmluZWQ6dW5kZWZpbmVkOnVuZGVmaW5lZDp1bmRlZmluZWQ6ZW46eDg2OnVuZGVmaW5lZDp1bmRlZmluZWQ6 (Content-type: text/html, MD5: 5c3279782850da8c61960518d4f6fcd9)
[2015-05-08 00:42:47] Saving log analysis at ../logs/6b3804e98f36d15e1bb5bc1e9cee0a75/20150508004242
{% endhighlight %}

Checking out the logs directy shows us some long directory names. There is the thug.csv file which tells you which url request corresponds to the directory names. The names were computed by hashing the contents of the request.

{% highlight tcsh %}
honeydrive@honeydrive:/honeydrive/thug/logs/6b3804e98f36d15e1bb5bc1e9cee0a75$ tree
.
└── 20150508004242
    ├── analysis
    │   ├── graph.svg
    │   ├── json
    │   │   └── analysis.json
    │   └── maec11
    │       └── analysis.xml
    └── text
        └── html
            ├── 5c3279782850da8c61960518d4f6fcd9
            └── c0e703784116c3367d9454be61879d2b
{% endhighlight %}

unfortunately we didnt really get that much out of this request. 
looking at the html files it seems that the script is fingerprinting the requester and routing it to an exploit accordingly.


{% highlight tcsh %}
sudo python thug.py -u winxpie60 -J 1.1 http://192.168.5.105:8080/test
{% endhighlight %}

By playing with the user agent and the plugin versions you can get the webserver to behave differently. In fact you can see this happening real time on the webserver as it redirects based upon what you gave it.

The analysis folder in the log directly contains a generated image in svg format that allows to to visualize the redirects at each hop and the method of the redirect.









