A canopy of apple-blossom
===================================================

TL/DR: Many web applications can still easily be exploited to gain unauthorised access to sensitive data and webservers. Threats include SQL Injection, Code Injection, XSS, and Cookie poisoning.

These pages contain notes on techniques based on, and writeups of, Portswigger Labs and challenges and ctfs done in vulnerable web application lab environments.

----

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Weaponisation

   docs/prep/README.md
   Build a virtual local testlab <https://red.tymyrddin.dev/projects/iac/en/latest/docs/lab/README.html>
   docs/prep/proxies.md
   docs/prep/automate.md
   docs/prep/recognise.md
   docs/prep/extensions.md
   docs/prep/recon-scripts.md

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Reconnaissance

   docs/recon/README.md
   docs/recon/stroll.md
   Advanced searching <https://red.tymyrddin.dev/projects/recon/en/latest/docs/osint/searching.html>
   Google dorking <https://red.tymyrddin.dev/projects/recon/en/latest/docs/osint/google-dorking.html>
   docs/recon/dns.md
   docs/recon/sneaky.md
   docs/recon/techstack.md

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Enumeration

   docs/enum/README.md
   docs/enum/scanning.md
   docs/enum/binaries.md

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Notes on techniques

   docs/techniques/README.md
   docs/techniques/xss.md
   docs/techniques/redirects.md
   docs/techniques/clickjacking.md
   docs/techniques/csrf.md
   docs/techniques/idor.md
   docs/techniques/sqli.md
   docs/techniques/race.md
   docs/techniques/ssrf.md
   docs/techniques/id.md
   docs/techniques/xxe.md
   docs/techniques/cache.md
   docs/techniques/smuggling.md
   docs/techniques/ssti.md
   docs/techniques/traversal.md
   docs/techniques/auth.md
   docs/techniques/sso.md
   docs/techniques/acl.md
   docs/techniques/business.md
   docs/techniques/headers.md
   docs/techniques/sockets.md
   docs/techniques/rce.md
   docs/techniques/sop.md
   docs/techniques/disclosure.md
   docs/techniques/shells.md
   docs/techniques/jwt.md
   docs/techniques/pollution.md

----

.. image:: _static/images/portswigger.png
  :alt: Portswigger labs

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: XSS

   docs/xss/README.md
   docs/xss/1.md
   docs/xss/2.md
   docs/xss/3.md
   docs/xss/4.md
   docs/xss/5.md
   docs/xss/6.md
   docs/xss/7.md
   docs/xss/8.md
   docs/xss/9.md
   docs/xss/10.md
   docs/xss/11.md
   docs/xss/12.md
   docs/xss/13.md
   docs/xss/14.md
   docs/xss/15.md
   docs/xss/16.md
   docs/xss/17.md
   docs/xss/18.md
   docs/xss/19.md
   docs/xss/20.md
   docs/xss/21.md
   docs/xss/22.md
   docs/xss/23.md
   docs/xss/24.md
   docs/xss/25.md
   docs/xss/26.md
   docs/xss/27.md
   docs/xss/28.md
   docs/xss/29.md
   docs/xss/30.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: SQLi

   docs/sqli/README.md
   docs/sqli/1.md
   docs/sqli/2.md
   docs/sqli/3.md
   docs/sqli/4.md
   docs/sqli/5.md
   docs/sqli/6.md
   docs/sqli/7.md
   docs/sqli/8.md
   docs/sqli/9.md
   docs/sqli/10.md
   docs/sqli/11.md
   docs/sqli/12.md
   docs/sqli/13.md
   docs/sqli/14.md
   docs/sqli/15.md
   docs/sqli/16.md
   docs/sqli/17.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: CSRF

   docs/csrf/README.md
   docs/csrf/1.md
   docs/csrf/2.md
   docs/csrf/3.md
   docs/csrf/4.md
   docs/csrf/5.md
   docs/csrf/6.md
   docs/csrf/7.md
   docs/csrf/8.md
   docs/csrf/9.md
   docs/csrf/10.md
   docs/csrf/11.md
   docs/csrf/12.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Clickjacking

   docs/clickjacking/README.md
   docs/clickjacking/1.md
   docs/clickjacking/2.md
   docs/clickjacking/3.md
   docs/clickjacking/4.md
   docs/clickjacking/5.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: DOM-based vulns

   docs/dom/README.md
   docs/dom/1.md
   docs/dom/2.md
   docs/dom/3.md
   docs/dom/4.md
   docs/dom/5.md
   docs/dom/6.md
   docs/dom/7.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: CORS

   docs/cors/README.md
   docs/cors/1.md
   docs/cors/2.md
   docs/cors/3.md
   docs/cors/4.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: XXE

   docs/xxe/README.md
   docs/xxe/1.md
   docs/xxe/2.md
   docs/xxe/3.md
   docs/xxe/4.md
   docs/xxe/5.md
   docs/xxe/6.md
   docs/xxe/7.md
   docs/xxe/8.md
   docs/xxe/9.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: SSRF

   docs/ssrf/README.md
   docs/ssrf/1.md
   docs/ssrf/2.md
   docs/ssrf/3.md
   docs/ssrf/4.md
   docs/ssrf/5.md
   docs/ssrf/6.md
   docs/ssrf/7.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: HTTP request smuggling

   docs/smuggling/README.md
   docs/smuggling/1.md
   docs/smuggling/2.md
   docs/smuggling/3.md
   docs/smuggling/4.md
   docs/smuggling/5.md
   docs/smuggling/6.md
   docs/smuggling/7.md
   docs/smuggling/8.md
   docs/smuggling/9.md
   docs/smuggling/10.md
   docs/smuggling/11.md
   docs/smuggling/12.md
   docs/smuggling/13.md
   docs/smuggling/14.md
   docs/smuggling/15.md
   docs/smuggling/16.md
   docs/smuggling/17.md
   docs/smuggling/18.md
   docs/smuggling/19.md
   docs/smuggling/20.md
   docs/smuggling/21.md
   docs/smuggling/22.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: OS command injection

   docs/os/README.md
   docs/os/1.md
   docs/os/2.md
   docs/os/3.md
   docs/os/4.md
   docs/os/5.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: SSTI

   docs/ssti/README.md
   docs/ssti/1.md
   docs/ssti/2.md
   docs/ssti/3.md
   docs/ssti/4.md
   docs/ssti/5.md
   docs/ssti/6.md
   docs/ssti/7.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Directory traversal

   docs/traversal/README.md
   docs/traversal/1.md
   docs/traversal/2.md
   docs/traversal/3.md
   docs/traversal/4.md
   docs/traversal/5.md
   docs/traversal/6.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Access control vulnerabilities

   docs/acl/README.md
   docs/acl/1.md
   docs/acl/2.md
   docs/acl/3.md
   docs/acl/4.md
   docs/acl/5.md
   docs/acl/6.md
   docs/acl/7.md
   docs/acl/8.md
   docs/acl/9.md
   docs/acl/10.md
   docs/acl/11.md
   docs/acl/12.md
   docs/acl/13.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Authentication

   docs/auth/README.md
   docs/auth/1.md
   docs/auth/2.md
   docs/auth/3.md
   docs/auth/4.md
   docs/auth/5.md
   docs/auth/6.md
   docs/auth/7.md
   docs/auth/8.md
   docs/auth/9.md
   docs/auth/10.md
   docs/auth/11.md
   docs/auth/12.md
   docs/auth/13.md
   docs/auth/14.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Websockets

   docs/sockets/README.md
   docs/sockets/1.md
   docs/sockets/2.md
   docs/sockets/3.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Web cache poisoning

   docs/cache/README.md
   docs/cache/1.md
   docs/cache/2.md
   docs/cache/3.md
   docs/cache/4.md
   docs/cache/5.md
   docs/cache/6.md
   docs/cache/7.md
   docs/cache/8.md
   docs/cache/9.md
   docs/cache/10.md
   docs/cache/11.md
   docs/cache/12.md
   docs/cache/13.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Insecure deserialisation

   docs/deserialisation/README.md
   docs/deserialisation/1.md
   docs/deserialisation/2.md
   docs/deserialisation/3.md
   docs/deserialisation/4.md
   docs/deserialisation/5.md
   docs/deserialisation/6.md
   docs/deserialisation/7.md
   docs/deserialisation/8.md
   docs/deserialisation/9.md
   docs/deserialisation/10.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Information disclosure

   docs/id/README.md
   docs/id/1.md
   docs/id/2.md
   docs/id/3.md
   docs/id/4.md
   docs/id/5.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Business logic vulnerabilities

   docs/business/README.md
   docs/business/1.md
   docs/business/2.md
   docs/business/3.md
   docs/business/4.md
   docs/business/5.md
   docs/business/6.md
   docs/business/7.md
   docs/business/8.md
   docs/business/9.md
   docs/business/10.md
   docs/business/11.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: HTTP Host header attacks

   docs/headers/README.md
   docs/headers/1.md
   docs/headers/2.md
   docs/headers/3.md
   docs/headers/4.md
   docs/headers/5.md
   docs/headers/6.md
   docs/headers/7.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: OAuth authentication

   docs/oauth/README.md
   docs/oauth/1.md
   docs/oauth/2.md
   docs/oauth/3.md
   docs/oauth/4.md
   docs/oauth/5.md
   docs/oauth/6.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: File upload vulnerabilities

   docs/upload/README.md
   docs/upload/1.md
   docs/upload/2.md
   docs/upload/3.md
   docs/upload/4.md
   docs/upload/5.md
   docs/upload/6.md
   docs/upload/7.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: JWT

   docs/jwt/README.md
   docs/jwt/1.md
   docs/jwt/2.md
   docs/jwt/3.md
   docs/jwt/4.md
   docs/jwt/5.md
   docs/jwt/6.md
   docs/jwt/7.md
   docs/jwt/8.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Prototype pollution

   docs/pollution/README.md
   docs/pollution/1.md
   docs/pollution/2.md
   docs/pollution/3.md
   docs/pollution/4.md
   docs/pollution/5.md
   docs/pollution/6.md
   docs/pollution/7.md
   docs/pollution/8.md
   docs/pollution/9.md
   docs/pollution/10.md

----

.. image:: _static/images/ctf.png
  :alt: CTFs and challenges

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: RootMe

   docs/rootme/README.md
   docs/rootme/git.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: TryHackMe

   docs/thm/README.md
   docs/thm/picklerick.md
