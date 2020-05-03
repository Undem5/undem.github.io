---
layout: post
title: "VulUniversity"
date:   2014-01-27 16:57:51
---

First step for this website under construction.

We will discover some basic tools such as
* Nmap
* Dirsearch
* Burp Intruder

# Nmap
Scanning target is the easiest part
{% highlight bash %}
nmap ipaddresss
nmap -iL file_full_of_ip
{% endhighlight %}

Nmap will scan the first 1024 default ports so you will have to specify
{% highlight bash %}
 -p : which port you decide to scan
 -p- or -p 1-65535 : all port
{% endhighlight %}


There are many scanning methods:
{% highlight bash %}
-sS : stealth scan (send just SYN packets)
-sT : TCP Connect scan (send SYN and ACK packets, less stealthy since the etablishment of a connection will be present in the logs of the target host
-sU : UDP scan (send UDP packets, may be useful)
-sF/-sN/-sX : TCP FIN/NULL/Xmas the difference is the flag value in the header of the packets. Can sneak through some non-stateful firewalls
{% endhighlight %}

You can use the OS detection:
{% highlight bash %}
 -O
{% endhighlight %}

But also the service detection:
{% highlight bash %}
 -sV
{% endhighlight %}

Nmap comes with NSE scripts:
{% highlight bash %}
 --script
 --script-updatedb: to update the database
 -sC : default NSE scripts
 --script=http, banner, : you can combine multiple scripts
{% endhighlight %}
The NSE scripts are on your local machine

For timing and performances:

{% highlight bash %}
 --scan-delay <ms>: adjust delay between two packets send. It may be useful for to prevent IDS detection
 -T <Paranoid 0|Sneaky 1|Polite 2|Normal 3|Aggressive 4|Insane 5> : from IDS evasion to speed scans (for fast networks for example)
{% endhighlight %}

Finally the output format:
You can combine nmap output with Metasploit. If you are using Metasploit, you can use db_nmap with

{% highlight bash %}
 use auxiliary/scanner/portscan/... (syn/tcp/...)
{% endhighlight %}

Otherwise you can export in XML format 

{% highlight bash %}
 -oX : XML output
 -oG : Gregable output
{% endhighlight %}

Here is an example
![a nmap example](/assets/vulnuniversity/nmap.png)
<img src="{{ base.url }}/assets/img/vulnuniversity/nmap.png">
