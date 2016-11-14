---
layout: post
title:  "Scheduling Powershell scripts that have input values"
date:   2015-11-23 09:19:00
categories: powershell windows
---


Simple powershell scripts are scheduled using the following format:

{% highlight text %}
powershell.exe -file "C:\blah\foo\thescript.ps1"
{% endhighlight %}

Powershell scripts that have arguments are scheduled using the -Command argument with '&' script invokation occuring within the inline command:

{% highlight text %}
powershell.exe -Command "& 'c:\fso\ScriptThatAcceptsTwoInputParameters.ps1' -TheFirstParam 'Blah' -TheSecondParam 'Meh'"
{% endhighlight %}
