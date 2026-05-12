# User and machine accounts

User accounts:
- Local systems (not joined in AD)
- Domai accounts (joined in AD)

When a user logs in, the system verifies their password and creates an access token. This token descripbes:
- thread
- user's security identity and group membership


## Local Accounts

Local accounts are stored on a particular server or workstation. These can be assigned rights on that host or via group membership.


> Local User accounts are considered security principals but can only manage access to and secure resources on a "standalone host"

There are several default local user accounts:
- **Administrator:** The SID: `S-1-5-domain-500` 
- **Guest:** Disabled by default. The purpose: Allow users without an account on the computer to login temporarily with limited access rights. `blank password` by default.
- **SYSTEM:** OR `NT AUTHORITY\SYSTEM` is a default account installed and used by the OS to perform many *internal functions*. This account doesn't have a profile but it has permission over everything. 
- **Network Service:** Predefined local account used by "Service Control manager (SCM)" for running *windows services.* When a service runs in the context of this particular account, it will present credentials to *remote services.*
- **Local Service:** Predefined local account used by SCM. It is configured with *minimal privileges* on the computer and presents *anonymous creds* to the network.

## Domain Users

Domain users are granted rights from the domain to access resources.
- They can log in any host in the domain.
- `KRBTGT`: This is a type of local account built into AD infrastructure. This acts as a *service account* for the `Key Distribution service` providing authentication and access for domain resources.

> `KRBTGT` is a common target. It can be leveraged for privilege escalation and persistence in a domain through attacks e.g "Golden Ticket"

## User Naming Attributes

- `UserPrincipalName (UPN):` Primary logon namefor the user. By convention: email address.
- `ObjectGUID:` unique identifier of the user.
- `SAMAccountName:` This is a logon name that supports the previous version of windows clients and servers.
- `ObjectSID:` The user's security identifier (SID).
- `sIDHistory:` This contains *previosu SIDs* for the user object especially in migration scenarios from domain to domain. The last SID -> `SIDHistory`. New SID -> `ObjectSID`.

```
Get-ADUser -Identity htb-student
```

### Domain Joined Machines

A host joined to a domain will acquire any configurations or changes necessary through the domain's Group policy.
The benefit:A user in the domain can log in and access resources from any host joined to the domain.

### Non-domain Joined Machines

Non Domain joined computers or computers in a `workgroup` are not managed by `domain policy`. 
the benefit: Individual users are in charge of any changes they with to make to their host.

> A machine account (`NT AUTHORITY\SYSTEM`) will have the most of the same rights as a standard "Domain User account". 