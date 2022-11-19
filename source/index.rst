A canopy of apple-blossom
===================================================

TL/DR: Many web applications can still easily be exploited to gain unauthorized access to sensitive data and web
servers. Threats include SQL Injection, Code Injection, XSS, and Cookie poisoning.

And if not for that and shared hosting is used, there may be access via a vulnerable neighbour. Or maybe the server
itself is vulnerable (unlikely, but one never knows).

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Information gathering

   docs/recon/README.md
   docs/recon/frontend.md
   docs/recon/dns.md
   docs/recon/fingerprint-webserver.md
   docs/recon/crawl-site.md
   docs/recon/app.md
   docs/recon/databases.md
   docs/recon/mapping.md

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Enumeration

   docs/enum/README.md
   docs/enum/scanning.md
   docs/enum/webserver.md
   docs/enum/input-vectors.md
   docs/enum/xss.md
   docs/enum/ssrf.md
   docs/enum/creative.md

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Exploitation

   docs/exploit/README.md
   docs/exploit/file-upload.md
   docs/exploit/rce.md
   docs/exploit/lfi.md
   docs/exploit/rfi.md
   docs/exploit/xss.md
   docs/exploit/hook-to-beef.md
   docs/exploit/cookies.md
   docs/exploit/clickjacking.md
   docs/exploit/csrf.md
   docs/exploit/last-resort.md
   docs/exploit/mitb.md
   docs/exploit/browser-based.md

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

   Red Team <https://tymyrddin.github.io/red/>
