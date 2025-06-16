---
layout: post
title: "XML Filtering Windows Events"
date:   2025-06-14 10:00:00
---

# Filtering with XML in the Windows Event Viewer
You can perform custom XML search query to look for specific Event ID
{% highlight xml %}
<Query Id="0" Path="Security">
    <Select Path="Security">
      *[System[(EventID=4624) and (EventRecordID=2677922)]]
    </Select>
 </Query>
{% endhighlight %}

Here is an example
<img src="{{ site.baseurl }}/assets/img/winevent/search1.PNG">

# Filtering Windows Events with XML in PowerShell

{% highlight powershell %}
$xmlQuery = @'
>> <QueryList>
>>   <Query Id="0" Path="Security">
>>     <Select Path="Security">
>>       *[System[(EventID=4624) and (EventRecordID=2677922)]]
>>     </Select>
>>   </Query>
>> </QueryList>
>> '@

Get-WinEvent -FilterXml $xmlQuery | Format-List *
{% endhighlight %}

And to put it in syslog format so better analysis

{% highlight powershell %}
$eventLogs = Get-WinEvent -FilterXml $xmlQuery | Select-Object *

foreach ($event in $eventLogs) {
>>     $syslogFormat = @"
>> {
>>   "timestamp": "$($event.TimeCreated.ToString("yyyy-MM-ddTHH-mm-ss.fffZ") -replace "`r`n", "\r\n\r\n")",
>>   "event_id": "$($event.Id -replace "`r`n", "\r\n\r\n")",
>>   "provider": "$($event.ProviderName -replace "`r\n", "\r\n\r\n")",
>>   "record_id": "$($event.RecordId -replace "\s+", "\r\n\r\n")",
>>   "message": "$($event.Message -replace "`r`n", "\r\n\r\n")"
>> }
>> "@
>>     Write-Output $syslogFormat
>> }
>>

{% endhighlight %}

Here is an output example:
