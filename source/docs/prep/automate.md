# Vulnerability discovery and exploitation

These can save time and are perfect for so-called juicy bugs, such as XSS, SQL injections, cross-site request forgery (CSRF), and other kinds of injections.

* [Nikto](https://cirt.net/Nikto2) is like a vulnerability scanner, but it is very limited. Nikto uses signatures to detect vulnerabilities, which means that Nikto can only detect well-known vulnerabilities. But it does not mean that is not useful; there are a lot of companies in the world using vulnerable open-source and private applications.
* [sqlmap](http://sqlmap.org/) is a great tool for exploiting SQL injections. Everyone who has exploited an SQL injection in the old-school style will know how complex and how slow this can be. You need to know about not just SQL, but also all the different syntaxes between all the DBMSs, the different tricks to extract information, DBMS configuration, etc. Sqlmap automatises the exploitation, and it can also detect vulnerabilities in the application.
* CO2 is a Burp Suite extension that helps launch sqlmap using an HTTP request from Burp Suite, using sessions, cookies, and more. A great collection of tools.
