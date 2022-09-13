# Authentication Relays

We will use Responder to perform a Man-in-the-Middle attack to intercept and poison the NetNTLM authentication requests broadcasted on the network.

By poisoning these requests, Responder attempts to force the client to connect to us. In the same line, it starts to host several servers such as SMB, HTTP, SQL, and others to capture these requests and force authentication.

`sudo responder -I INTERFACE -dwv`

Then crack the NetNTLM hash offline with hashcat:

`hashcat -m 5600 <hash file> <password file> --force`

# Answers


What is the name of the tool we can use to poison and capture authentication requests on the network?
`Responder`

What is the username associated with the challenge that was captured?
`svcFileCopy`

What is the value of the cracked password associated with the challenge that was captured?
`FPassword1!`