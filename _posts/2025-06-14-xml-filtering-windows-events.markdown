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
<img src="{{ site.baseurl }}/assets/img/winevent/search1.png">