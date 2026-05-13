# Active directory Rights and Privileges

Rights are assigned to users or groups and deal with permissions to access an object.
Privileges grant a user permission to perform an action e.g run a program, reset passwords etc.

## Built in AD Groups

- `Account Operators` : Members can create and modify most types of accounts and can log in locally to `domain controller`. 
They cannot manage admin accounts, members of admins, server operators, account operators, backup operators or print operators groups.

- `Administrators` : Members have full and unrestricted access to a computer or an entire domain if they are in this group on a domain controller.

- `Backup operators` : Members can restore all files on a computer, regardless of the permissions on the files. They can log on and shut down the computer. They can log on to DCs locally and considered Domain admins. 
They can make shadow copies of the `SAM/NTDS` database which can be used to extract credentials.

- `DnsAdmins` : Members have access to **Network DNS information**. 

- `Domain Admins` : Members have full access to administer the domain and are members of local administrator's group on all domain joined machines.

- `Domain Computers` : Any computers created in the domain aside from DC are added to this group.

- `Domain Controllers` : Contains all DCs within a domain. New DCs are added to this group automatically.

- `Domain Guests` : The domain's built in guest account. They have profile created when signing into a domain joined computer as a local guest.

- `Domain Users` : This group contains all the user accounts in a domain.

- `Enterprise Admins` : Membership in this group provides *complete configuration access* within the domain. This group only exists in the `root domain` of an AD forest. They can change forest wide and "The administrator" account is the member of this group by default.

- `Event Log Readers` : Members can read event logs on local computers. The group is created when "A host is promoted to a domain controller"

- `Group Policy Creator Owners` : Members create, edit or delete GPOs in the domain. 

- `Hyper-V Administrators` : Members have access to all the features in Hyper-v.

- `IIS_IUSRS` : A built-in gruop used by Internet Information Services.

- `Pre-windows 2000 Compatible Access` : Members in this group is often a leftover legacy configuration.

- `Print Operators` : Members can manage, create, share and delete printers that are connected to domain controllers in the domain. They are allowed to log on DCs locally and may be used to load a malicious printer driver and escalate privileges within the domain.

- `Protected Users` : Members of this group are provided additional protections against credential theft and tactics e.g kerberos abuse.

- `Read-only Domain Controllers` : Contains all Read-only domain controllers in the domain.

- `Remote Desktop Users` : This group is used to grant users and groups permission to connect to a host via RDP. This group cannot be renamed, deleted or moved.

- `Remote management users` : This group can be used to grant users remote access to computers via winrm. 

- `Schema Admins` : Members can modify the AD schema which is the way all objects with AD are defined. Only exists in root domain of an AD forest. The administrator is the onlly member by default.

- `Server Operators` : This group exists on domain controllers. Members can modify services, access SMB shares and backup files on domain controllers. By default, it has no members.

```
Get-ADGroup -Identity "Server Operators" -Properties *
```

**Domain Admisn Group Membership**
```
Get-ADGroup -Identity "Domain Admins" -Properties * | select DistinguishedName,GroupCategory,GroupScope,Name,Members
```

## User Rights Assignment

Users can have various rights assigned to their account. More info:
 https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/user-rights-assignment

- `SeRemoteInteractiveLogonRight` : It can give our target user the right to log onto a host via RDP
- `SeBackupPrivilege` : It grants a user the ability to create *System backups* and could be obtained copies of sensitive files that can be used to retrieve passwords such as `SAM` and `SYSTEM Registry hives` and `NTDS.DIT`.
- `SeDebugPrivilege` : Allows a user to debug and adjust the memory of a process. Tool such as `Mimikatz` to read the memory space of the Local System AUthority (LSASS) to obtain creds stored in memoroy.
- `SeImpersonatePrivilege` : Allows to impersonate a token of a privileged account e.g `NT AUTHORITY\SYSTEM`. Can be leveraged through `JuicyPotato`, `RogueWinRm`, `PrintSpoofer` to escalate privileges.
- `SeLoadDrivePrivilege` : Allows load and unload device drivers that could potentially be used to escalate or compromise a system.
- `SeTakeOwnershipPrivilege` : It allows a process to take ownership of an object.We could use this to gain access to a file share.

### Domain Admin Rights Non-elevated

By default, windows systems do not enable rights to us uneless we run the CMD or powershell console. This is controlled by `User Account Control (UAC)`.

```
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ==================================== ========
SeShutdownPrivilege           Shut down the system                 Disabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
SeUndockPrivilege             Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
SeTimeZonePrivilege           Change the time zone                 Disabled

```

### Domain Admin Right Elevated

```
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State
========================================= ================================================================== ========
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Disabled
SeMachineAccountPrivilege                 Add workstations to domain                                         Disabled
SeSecurityPrivilege                       Manage auditing and security log                                   Disabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Disabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Disabled
SeSystemProfilePrivilege                  Profile system performance                                         Disabled
SeSystemtimePrivilege                     Change the system time                                             Disabled
SeProfileSingleProcessPrivilege           Profile single process                                             Disabled
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Disabled
SeCreatePagefilePrivilege                 Create a pagefile                                                  Disabled
SeBackupPrivilege                         Back up files and directories                                      Disabled
SeRestorePrivilege                        Restore files and directories                                      Disabled
SeShutdownPrivilege                       Shut down the system                                               Disabled
SeDebugPrivilege                          Debug programs                                                     Enabled
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Disabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled
SeRemoteShutdownPrivilege                 Force shutdown from a remote system                                Disabled
SeUndockPrivilege                         Remove computer from docking station                               Disabled
SeEnableDelegationPrivilege               Enable computer and user accounts to be trusted for delegation     Disabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Disabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled
SeCreateGlobalPrivilege                   Create global objects                                              Enabled
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Disabled
SeTimeZonePrivilege                       Change the time zone                                               Disabled
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Disabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Disabled
```

