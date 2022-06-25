# manage-windows-ansible

Configure Windows Server

1-Check the PowerShell Version:

$PSVersionTable


2-Check the version of .NET installed:

Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse | Get-ItemProperty -Name version -EA 0 | Where { $_.PSChildName -Match '^(?!S)\p{L}'} | Select PSChildName, version


3-To view the current listeners that are running on the WinRM service, run the following command:

winrm enumerate winrm/config/Listener


4-Note: If you do not see the information of the listeners, run following script in PowerShell to configure listener and service for the WinRM. (This script script sets up both HTTP and HTTPS listeners with a self-signed certificate and enables the Basic authentication option on the service.)


[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"

(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)

powershell.exe -ExecutionPolicy ByPass -File $file