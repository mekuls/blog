---
layout: post
title:  "Managing Office365 and Azure Active Directory using Powershell"
date:   2015-11-23 09:19:00
categories: azure powershell 
---

Managing Azure stuff using Powershell is awesome. There are a few prerequisites required in order to run commands.

- First install this [https://www.microsoft.com/en-us/download/details.aspx?id=41950]
- Then install this [http://go.microsoft.com/fwlink/p/?linkid=236297]

Then you're pretty much good to go.

##A quick note:
I decided once upon a time to try to invoke the Azure cmdlets from within an Aure WebApp instance. This is a bad idea because you'll need to

##Another quick note:
The following snippet illustrates how to manage credentials before calling the Connect-MsolService cmdlet

{% highlight powershell %}

$adminLoginName= "admin@something.com"
$adminLoginPassword = "MySecret"


$secpasswd = ConvertTo-SecureString $adminLoginPassword -AsPlainText -Force
$adminLoginCredential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList @($adminLoginName, $secpasswd)

Import-Module MSOnline
Connect-MSOLservice -Credential $adminLoginCredential
 
{% endhighlight %}