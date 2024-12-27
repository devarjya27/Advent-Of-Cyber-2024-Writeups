Today we learn about `Active Directories` sturctures and learn about various `Active Directory` attacks.

`Active Directory` is a Directory Service at the heart of most enterprise networks that stores information about objects in a network.

The building blocks of an AD architecture include:

+ Domains: Logical groupings of network resources such as users, computers, and services. They serve as the main boundary for AD administration and can be identified by their Domain Component and Domain Controller name. Everything inside a domain is subject to the same security policies and permissions.
+ Organisational Units (OUs): OUs are containers within a domain that help group objects based on departments, locations or functions for easier management. Administrators can apply Group Policy settings to specific OUs, allowing more granular control of security settings or access permissions.
+ Forest: A collection of one or more domains that share a standard schema, configuration, and global catalogue. The forest is the top-level container in AD.
+ Trust Relationships: Domains within a forest (and across forests) can establish trust relationships that allow users in one domain to access resources in another, subject to permission.

Types of `Active Directory` attacks:

+ Golden Ticket Attack: A Golden Ticket attack allows attackers to exploit the Kerberos protocol and impersonate any account on the AD by forging a Ticket Granting Ticket (TGT).
+ Pass-The-Hash: This type of attack steals the hash of a password and can be used to authenticate to services without needing the actual password.
+ Kerberoasting: Kerberoasting is an attack targeting Kerberos in which the attacker requests service tickets for accounts with Service Principal Names
+ Pass-the-Ticket: In a Pass-the-Ticket attack, attackers steal Kerberos tickets from a compromised machine and use them to authenticate as the user or service whose ticket was stolen.
