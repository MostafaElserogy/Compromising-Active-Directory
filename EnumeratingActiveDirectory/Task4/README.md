# Enumeration through Command Prompt

You can user the `net` command to list all users in the AD domain by
using the `user` sub-option:
`net user /domain`

We can use the `net` command to enumerate the groups of the domain by using the `group` sub-option:
`net group /domain`

We can use the `net` command to enumerate the password policy of the domain by using the `accounts` sub-option:

## Answers
Apart from the Domain Users group, what other group is the aaron.harris account a member of?
`Internet Access`

Is the Guest account active? (Yay,Nay)
`Nay`

How many accounts are a member of the Tier 1 Admins group?
`7`

What is the account lockout duration of the current password policy in minutes?
`30`