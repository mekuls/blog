Powershell has an awesome set of cmdlets that came with version [Todo: Add version here] that allow you to schedule and run Jobs.

Let's say we have 3 scripts. 
1. A script that creates a resource
2. A script that consumes the resource creates in step 1. 
3. A command script which executes scripts 1 and 2.

Script1: Job1_EnumProcesses.ps1
{% highlight powershell %}
$csvFileName = "Processes.csv"

Write-Output "[+] Running Job #1"
$processes = Get-Process
$processes = $processes | Export-Csv $csvFileName -NoTypeInformation
{% endhighlight }

Script2: Job2_PrintProcesses.ps1
{% highlight powershell %}
$csvFileName = "Processes.csv"

Write-Output "[+] Running Job #2"
$processes = Import-CSV $csvFileName

$processesToPrint = $processes | Select -First 5

foreach ($proc in $processes)
{
    Write-Output "[+] found process: $($proc.Name)"
}
{% endhighlight %}


Script3: Control.ps1
{% highlight powershell %}
Write-Output "[+] Job Runner"

$script1 = Join-Path -Path $PWD -ChildPath "Job1_EnumProcesses.ps1"
$script2 = Join-Path -Path $PWD -ChildPath "Job2_PrintProcesses.ps1"


Start-Job -File $script1
Start-Job -File $script2
{% endhighlight %}

The output of the job runner looks like this:
{% highlight text %}
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Job1            BackgroundJob   Completed     True            localhost            Write-Output "[+] Runn...
3      Job3            BackgroundJob   Completed     True            localhost            $csvFileName = "Proces...
5      Job5            BackgroundJob   Completed     True            localhost            $csvFileName = "Proces...
{% endhighlight %}

Which is not very helpful