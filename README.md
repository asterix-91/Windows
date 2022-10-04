# Windows


```
SCAN ICMP 
nmap -sP 10.10.10.0/24

SCAN TCP
nmap 10.10.10.0/24

SCAN SYN
nmap -sS 10.10.10.0/24

SCAN NetBIOS
nbtscan 10.10.0.0/16
```
```
Requete LDAP
ldapsearch -x -H ldap://dc01.medic.ex -s base -LLL
```
```
DNS KDC
nslookup -type=SRV _kerberos._tcp.medic.ex
```

```
SCAN AC
certipy find 'medic.ex/pixis:P4ssw0rd@dc01.medic.ex'
```

```
Get-ADDefaultDomainPasswordPolicy
```

```
Get-ADObject -LDAPFilter "(|(ObjectClass=user)(ObjectClass=computer))" -SearchBase "DC=MEDIC,DC=EX" -Property * |  where description -ne $null | Select Name, Description, ObjectClass
```
