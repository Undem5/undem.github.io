---
layout: post
title: "VulnUniversity"
date:   2014-01-27 16:57:51
---

#Nmap - First step scanning the host
<img src="{{ site.baseurl }}/assets/img/vulnuniversity/nmap.png">


Then we will use dirsearch to search for web directories

# Dirsearch - Web path scanner

We found a directory named "internal"
This is a web page to upload file but we don't know the extension...

Let's use Burp Intruder

# Burp Intruder - Test file extensions
We choose simple list and we upload a common list of extensions
On positions, we choose the attack type: Sniper.
When we are on the web page, we launch the Intruder option:


<img src="{{ site.baseurl }}/assets/img/vulnuniversity/burp_proxy.png">
with the Forward button

And we get:


<img src="{{ site.baseurl }}/assets/img/vulnuniversity/burp_intruder.png">
So we create a phtml reverse shell and we upload the file. At the same time, we use netcat
to listen for the incoming connection:

<img src="{{ site.baseurl }}/assets/img/vulnuniversity/netcat.png">

# SUID permission

We search for Set-UID permission:
{% highlight bash %}
 find / -perm /40000
{% endhighlight %}
(Btw SGID is 2000 and so SGID+SUID=6000)

We found the binary systemctl.
The flag is at
{% highlight bash %}
  /root/flag.txt
{% endhighlight %}

So we create a service which will display the content of the file.

But we don't have the permission to create a file in the systemd directory. How to do it? Environment variable:

# Systemctl Link feature
We save the service conf file in an environment variable.
Systemctl has an interesting option:
{% highlight bash %}
  systemctl link path_to_unit_file
{% endhightlight %}
You can link a unit file which will create a link in /etc/systemd/system.
You just need to enable it and start it. By combining the option 
{% highlight bash %}
  enable --now: When used with enable, the units will also be started
{% endhighlight %}

And then we get the content of the file we want:

<img src="{{ site.baseurl }}/assets/img/vulnuniversity/systemctl.png">

Hoped you liked this article!
