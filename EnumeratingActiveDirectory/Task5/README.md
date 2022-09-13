# Enumeration through PowerShell

## Users
We can use the `Get-ADUser` cmdlet to enumerate AD users:
`Get-ADUser -Identity gordon.stevens -Server za.tryhackme.com -Properties *`

```shell
The parameters are used for the following:
	-Identity - The account name that we are enumerating
	-Properties - Which properties associated with the account will be shown, * will show all properties
	-Server - Since we are not domain-joined, we have to use this parameter to point it to our domain controller
```

For most of these cmdlets, we can also use the `-Filter` parameter that allows more control over enumeration and use the `Format-Table` cmdlet to display the results such as the following neatly:
`Get-ADUser -Filter 'Name -like "*stevens"' -Server za.tryhackme.com | Format-Table`

## Groups
We can use the `Get-ADGroup` cmdlet to enumerate AD groups:
`Get-ADGroup -Identity Administrators -Server za.tryhackme.com`

## AD Objects
A more generic search for any AD objects can be performed using the `Get-ADObject` cmdlet. For example, if we are looking for all AD objects that were changed after a specific date:
`New-Object DateTime(2022, 02, 28, 12, 00, 00)`
`Get-ADObject -Filter 'whenChanged -gt $ChangeDate' -includeDeletedObjects -Server za.tryhackme.com`

If we wanted to, for example, perform a password spraying attack without locking out accounts, we can use this to enumerate accounts that have a badPwdCount that is greater than 0, to avoid these accounts in our attack:
`Get-ADObject -Filter 'badPwdCount -gt 0' -Server za.tryhackme.com`

## Domains
We can use `Get-ADDomain` to retrieve additional information about the specific domain:
`Get-ADDomain -Server za.tryhackme.com`

## Altering AD Objects
we will show an example of this by force changing the password of our AD user by using the `Set-ADAccountPassword` cmdlet:
```Set-ADAccountPassword -Identity gordon.stevens -Server za.tryhackme.com -OldPassword (ConvertTo-SecureString -AsPlaintext "old" -force) -NewPassword (ConvertTo-SecureString -AsPlainText "new" -Force)```
## Answers
What is the value of the Title attribute of Beth Nolan (beth.nolan)?
`senior`

What is the value of the DistinguishedName attribute of Annette Manning (annette.manning)?
`CN=annette.manning,OU=Marketing,OU=People,DC=za,DC=tryhackme,DC=com`

When was the Tier 2 Admins group created?
`2/24/2022 10:04:41 PM`

What is the value of the SID attribute of the Enterprise Admins group?
`S-1-5-21-3330634377-1326264276-632209373-519`

Which container is used to store deleted AD objects?
`CN=Deleted Objects,DC=za,DC=tryhackme,DC=com`

