# Windows post exploitation enumeration

This info can help escalate privileges.

## System information

System information:

    systeminfo | findstr /b /C:"OS Name" /C"OS Version"

Check for installed updates:

    wmic qfe get Caption, Description

Check how many drivers are in the machine:

    wmic logicaldisk get Caption

## User information

Check current user:

    whoami

Check current user privileges:

    whoami /priv

Groups in which current user is involved:

    whoami /groups

All users:

    net users

User information:

    net users <username>

List user groups:

    net localgroup

Members of a group

    net localgroup Administrators

## Network information

IP address configuration (default gateway, subnet, dns, domain controller):

    ipconfig /all

ARP table:

    arp -a 

Routing table:

    route print

Network status:

    net stat

## Password information

Search for the word "password" in text files:

    findstr /si password *.txt *.ini *.config

Searching for passwords in the registry:

```text
REG QUERY HKLM /F "password" /t REG_SZ /S /K 
REG QUERY HKCU /F "password" /t REG_SZ /S /K
```

Passwords in unattend files:

```text
C:\unattend.xml 
C:\Windows\Panther\Unattend.xml 
C:\Windows\Panther\Unattend\Unattend.xml 
C:\Windows\system32\sysprep.inf 
C:\Windows\system32\sysprep\sysprep.xml
```

## Firewall and AV information

Check Windows defender:

    sc query windefend

View all services running on the machine:

    sc queryex type= service

    sc = service control

Firewall settings:

    netsh advfirewall firewall dump

    netsh firewall show state

Show firewall config:

    netsh firewall show config