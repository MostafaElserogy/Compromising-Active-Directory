sudo openvpn --config ./spaceorion-breachingad.ovpn --daemon

# Brute-force Login Attacks

You have been provided with a list of usernames discovered during a red team OSINT exercise. The OSINT exercise also indicated the organisation's initial onboarding password, which seems to be "Changeme123". Although users should always change their initial password, we know that users often forget. We will be using a custom-developed script to stage a password spraying against the web application hosted at this URL: 
**http://ntlmauth.za.tryhackme.com.**

-----------------------------------------------------------------------------------------

We could use tools such as *Hydra* to assist with the password spraying attack. However, it is often better to script up these types of attacks yourself, which allows you more control over the process. A base python script has been provided in the task files that can be used for the password spraying attack. The following function is the main component of the script:

`hydra -L usernames.txt -p Changeme123 10.200.24.201 http-get`

OR

`python3 ntlm_passwordspray.py -u usernames.txt -a http://ntlmauth.za.tryhackme.com -p Changeme123 -f za.tryhackme.com`

# Answers

**
1. What is the name of the challenge-response authentication mechanism that uses NTLM?
`NetNtlm`

2. What is the username of the third valid credential pair found by the password spraying script?
`gordon.stevens`

3. How many valid credentials pairs were found by the password spraying script?
`4`

4. What is the message displayed by the web application when authenticating with a valid credential pair?
`Hello World`


