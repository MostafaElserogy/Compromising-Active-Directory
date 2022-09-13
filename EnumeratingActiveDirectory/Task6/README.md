# Enumeration through Bloodhound

## Sharphound

You will often hear users refer to Sharphound and Bloodhound interchangeably. However, they are not the same. Sharphound is the enumeration tool of Bloodhound. It is used to enumerate the AD information that can then be visually displayed in Bloodhound. Bloodhound is the actual GUI used to display the AD attack graphs. Therefore, we first need to learn how to use Sharphound to enumerate AD before we can look at the results visually using Bloodhound.

There are three different Sharphound collectors:
```shell
	-Sharphound.ps1 - PowerShell script for running Sharphound. However, the latest release of Sharphound has stopped releasing the Powershell script version. This version is good to use with RATs since the script can be loaded directly into memory, evading on-disk AV scans.
	-Sharphound.exe - A Windows executable version for running Sharphound.
	-AzureHound.ps1 - PowerShell script for running Sharphound for Azure (Microsoft Cloud Computing Services) instances. Bloodhound can ingest data enumerated from Azure to find attack paths related to the configuration of Azure Identity and Access Management.
```

When using these collector scripts on an assessment, there is a high likelihood that these files will be detected as malware and raise an alert to the blue team. This is again where our Windows machine that is non-domain-joined can assist. We can use the `runas` command to inject the AD credentials and point Sharphound to a Domain Controller. Since we control this Windows machine, we can either disable the AV or create exceptions for specific files or folders, which has already been performed for you on the THMJMP1 machine. You can find the Sharphound binaries on this host in the `C:\Tools\` directory. We will use the SharpHound.exe version for our enumeration, but feel free to play around with the other two. We will execute Sharphound as follows:

`Sharphound.exe --CollectionMethods <Methods> --Domain za.tryhackme.com --ExcludeDCs`

```shell
Parameters explained:
	-CollectionMethods - Determines what kind of data Sharphound would collect. The most common options are Default or All. Also, since Sharphound caches information, once the first run has been completed, you can only use the Session collection method to retrieve new user sessions to speed up the process.
	-Domain - Here, we specify the domain we want to enumerate. In some instances, you may want to enumerate a parent or other domain that has trust with your existing domain. You can tell Sharphound which domain should be enumerated by altering this parameter.
	-ExcludeDCs - This will instruct Sharphound not to touch domain controllers, which reduces the likelihood that the Sharphound run will raise an alert.
```

We can now use `Bloodhound` to ingest the generated ZIP to show us attack paths visually.

