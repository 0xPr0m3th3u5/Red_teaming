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

-`Pre-windows 2000 Compatible Access` : Members in this group is often a leftover legacy configuration.

- `Print Operators` : Members can manage, create, share and delete printers that are connected to domain controllers in the domain. They are allowed to log on DCs locally and may be used to load a malicious printer driver and escalate privileges within the domain.

- `Protected Users` : Members of this group are provided additional protections against credential theft and tactics e.g kerberos abuse.

- `Read-only Domain Controllers` : Contains all Read-only domain controllers in the domain.

- `Remote Desktop Users` : This group is used to grant users and groups permission to connect to a host via RDP. This group cannot be renamed, deleted or moved.

- `Remote management users` : This group can be used to grant users remote access to computers via winrm. 

- `Schema Admins` : Members can modify the AD schema which is the way all objects with AD are defined. Only exists in root domain of an AD forest. The administrator is the onlly member by default.

- `Server Operators` : This group exists on domain controllers. Members can modify services, access SMB shares and backup files on domain controllers. By default, it has no members.