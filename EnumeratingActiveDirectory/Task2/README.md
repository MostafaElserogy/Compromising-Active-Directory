# Credential Injection

In security assessments, you will often have network access and have just discovered AD credentials but have no means or privileges to create a new domain-joined machine. So we need the ability to use those credentials on a Windows machine we control.

If we have the AD credentials in the format of <username>:<password>, we can use Runas, a legitimate Windows binary, to inject the credentials into memory. The usual Runas command would look something like this:

`runas.exe /netonly /user:<domain>\<username> cmd.exe`

```shell
	-/netonly - Since we are not domain-joined, we want to load the credentials for network authentication but not authenticate against a domain controller. So commands executed locally on the computer will run in the context of your standard Windows account, but any network connections will occur using the account specified here.
	-/user - Here, we provide the details of the domain and the username. It is always a safe bet to use the Fully Qualified Domain Name (FQDN) instead of just the NetBIOS name of the domain since this will help with resolution.
	-cmd.exe - This is the program we want to execute once the credentials are injected. This can be changed to anything, but the safest bet is cmd.exe since you can then use that to launch whatever you want, with the credentials injected.
```
Now that we have injected our AD credentials into memory, this is where the fun begins. With the /netonly option, all network communication will use these injected credentials for authentication. This includes all network communications of applications executed from that command prompt window.

## Answers

What native Windows binary allows us to inject credentials legitimately into memory?
`runas.exe`

What parameter option of the runas binary will ensure that the injected credentials are used for all network connections?
`/netonly`

What network folder on a domain controller is accessible by any authenticated AD account and stores GPO information?
`SYSVOL`

When performing dir \\za.tryhackme.com\SYSVOL, what type of authentication is performed by default?
`Kerberos Authentication`