---
layout: post
title: Notes on HoneyD on Honeydrive
---


[http://bruteforce.gr/honeydrive](http://bruteforce.gr/honeydrive)

Honeydrive is a honeypot/net distribution solution. I have always wanted to run a public honeypot for malware collection but I will mostly be focusing on using it internally for spoofing whole networks. The objective is to build a full honeynet using honeyd.

Honeyd works through making configuration files (of which there are many examples) that control what virtualized systems honeyd will create.

The big issue with honeyd is getting the virtual systems to all connect properly.

Honeyd systems can be configured to to aquire an ip address using the dhcp service provided to the network. (In my case this was vmware's dhcp service)

Honeyd can also assign a static IP address in the configuration file to each system. This requires the use of the farpd command to spoof the arp table and funnel specific ip traffic to the honeypot.

Taking one more step and assigning an ip route to honeypot router with an entry and delegated subnet requires much more configuration




