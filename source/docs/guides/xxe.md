# XML external entity (XXE)

XML external entity attacks (XXEs) are fascinating vulnerabilities that target the XML parsers of an application. XXEs can be very impactful bugs, as they can lead to confidential information disclosure, SSRFs, and DoS attacks. But they are also difficult to understand and exploit.

* We can modify XML documents and define the document using a DTD. 
* If an entity is declared within a DTD it is called as internal entity.

```text
<!ENTITY entity_name "entity_value">
```

* If an entity is declared outside a DTD it is called as external entity. Identified by SYSTEM.

    `<!ENTITY entity_name SYSTEM "entity_value">`
  
* Parsers can resolve external references that could be displayed to the user.
* You can use [these XXE Templates](https://raw.githubusercontent.com/tymyrddin/scripts-webapp/main/resources/xxe-templates.md).

## Steps

1. Find data entry points that you can use to submit XML data.
2. Determine whether the entry point is a candidate for a classic or blind XXE. The endpoint might be vulnerable to classic XXE if it returns the parsed XML data in the HTTP response. If the endpoint does not return results, it might still be vulnerable to blind XXE, and you should
set up a callback listener for your tests.
3. Try out a few test payloads to see if the parser is improperly configured. In the case of classic XXEs, you can check whether the parser is processing external entities. In the case of blind XXEs, you can make the server send requests to your callback listener to see if you can trigger outbound interaction.
4. If the XML parser has the functionalities that make it vulnerable to XXE attacks, try to exfiltrate a common system file, like /etc/hostname.
5. You can also try to retrieve some more sensitive system files, like `/etc/shadow` or `~/.bash_history`.
6. If you cannot exfiltrate the entire file with a simple XXE payload, try to use an alternative data exfiltration method.
7. See if you can launch an SSRF attack using the XXE.
8. Draft up report.

## Escalation

What you can achieve with an XXE vulnerability depends on the permissions given to the XML parser. Generally, you can use XXEs to access and exfiltrate system files, source code, and directory listings on the local machine. You can also use XXEs to perform SSRF attacks to port-scan the target???s network, read files on the network, and access resources that are hidden behind a firewall. And, attackers sometimes use XXEs to launch DoS attacks.

## Resources

* [Portswigger: XML external entity (XXE) injection](https://portswigger.net/web-security/xxe)
* [OWASP: XML External Entity (XXE) Processing](https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing)
* [Bug Bounty Bootcamp](https://nostarch.com/bug-bounty-bootcamp)
* [Bug Bounty Hunting Essentials](https://www.packtpub.com/product/bug-bounty-hunting-essentials/9781788626897)
* [Bug Bounty Hunting for Web Security](https://link.springer.com/book/10.1007/978-1-4842-5391-5)

