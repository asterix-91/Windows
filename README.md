# Windows Active Directory Security


- Enumerate Domain Users
```powershell
#Get Users in a specific Domain 
Get-ADUser -server Domaincontroller -Filter * -Properties *
#Get Users wit PasswordNotRequired set to true
Get-ADUser -server Domaincontroller -Filter {PasswordNotRequired -eq $true}
#Get user's accounts that do not Require Kerberos Preauthentication 
Get-ADUser -Filter 'useraccountcontrol -band 4194304' -Properties useraccountcontrol | Format-Table name
#Enumerate user accounts with serverPrincipalName attribute set
Get-adobject -filter {serviceprincipalname -like "*"}  | Where-Object {$_.distinguishedname -like "*CN=Users*" -and $_.name -ne "krbtgt"}
```

- Enumerate Domain Computers
```powershell
#Get Computers in a specific Domain 
Get-ADComputer -server Domaincontroller -Filter * -Properties *
#Get all active computer list in domain
Get-ADComputer -Filter {enabled -eq $true} -properties *
#Get computers with outdated OS (in this exemple we are looking for computers on Windows 7)
Get-ADComputer -Filter 'operatingsystem -like "*Windows 7*" -and enabled -eq "true"' -Properties *
```
- Enumerate Domain Trust:
```powershell
#Get the list of all trusts within the current domain
Get-ADTrust -Filter *               
#Get the list of all trusts within the indicated domain
Get-ADTrust -Identity us.domain.corporation.local   
```
- Enumerate File Shares:
```powershell
#Retrieves the SMB shares on the computer
Get-SmbShare
```
- Other Objects Enumeration:
```powershell
#Shows the tickets in memory
klist
#Get the default domain password policy from a specified domain
Get-ADDefaultDomainPasswordPolicy -Identity corp.local.com
#Get all groups that contain the word "admin" in the group name
Get-ADGroup -Filter 'Name -like "*admin*"' | select Name     
#Get all members of the "Domain Admins" group
Get-ADGroupMember -Identity "Domain Admins" -Recursive       
#Get group membership for a specific user
Get-ADPrincipalGroupMembership -Identity login001     
```

- LDAPSHEARCH
```
#Get domain info with ldapsearch cmd without login
ldapsearch -x -H ldap://dc01.medic.ex -s base -LLL
````

- SCAN ICMP 
```shell
nmap -sP 10.10.10.0/24
```

- SCAN TCP
```
nmap 10.10.10.0/24
```

- SCAN SYN
```
nmap -sS 10.10.10.0/24
```

- SCAN NetBIOS
```
nbtscan 10.10.0.0/16
```

- Requete LDAP
```
ldapsearch -x -H ldap://dc01.medic.ex -s base -LLL
```

-DNS KDC
```
nslookup -type=SRV _kerberos._tcp.medic.ex
```

- SMBCLIENT
#List share accesible without login
```
smbclient -L //10.40.32.32 --no-pass
```
#connect to the share folder without login
```
smbclient //10.40.32.32/Replication --no-pass
```
#if you can login this is the cdm
```
smbclient //10.40.32.32/Replication -U user -W workgroup
```
#while connect to smb to dl all folder 
```
recurse on
prompt off
mget *
```
