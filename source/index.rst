A canopy of apple-blossom
===================================================

TL/DR: Many web applications can still easily be exploited to gain unauthorized access to sensitive data and web
servers. Threats include SQL Injection, Code Injection, XSS, and Cookie poisoning.

And if not for that and shared hosting is used, there may be access via a vulnerable neighbour. Or maybe the server
itself is vulnerable (unlikely, but one never knows).

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
   :caption: Detection and exploitation

   docs/exploit/README.md
   docs/exploit/xss.md
   docs/exploit/redirects.md
   docs/exploit/clickjacking.md
   docs/exploit/csrf.md
   docs/exploit/idor.md
   docs/exploit/sql.md
   docs/exploit/race.md
   docs/exploit/ssrf.md
   docs/exploit/id.md
   docs/exploit/xxe.md
   docs/exploit/ti.md
   docs/exploit/access.md
   docs/exploit/rce.md
   docs/exploit/sop.md
   docs/exploit/sso.md
   docs/exploit/disclosure.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Database

   docs/databases/README.md
   docs/databases/db-exploits.md
   docs/databases/sqli.md
   docs/databases/confirm-vuln.md
   docs/databases/login-bypass.md
   docs/databases/extract-info.md
   docs/databases/outside-www.md
   docs/databases/get-shell.md
   docs/databases/access-pwd-db.md
   docs/databases/crack-root.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Server

   docs/server/README.md
   docs/server/traversal.md
   docs/server/response-splitting.md
   docs/server/poison-cache.md
   docs/server/spoof.md
   docs/server/hijack-app-session.md
   docs/server/mitm.md
   docs/server/bruteforce.md

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Post-exploitation

   docs/post/README.md
   docs/post/overview.md
   docs/post/weevely.md
   docs/post/escalate-shell.md
   docs/post/privesc.md
   docs/post/linux-post.md
   docs/post/windows-post.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Capture The Flag

   docs/ctf/README.md
   docs/ctf/picklerick.md

.. toctree::
   :caption: Links

   Red Team <https://red.tymyrddin.dev/>
