# Application logic errors and broken access control

Application logic errors and broken access control vulnerabilities are a different beast. These are not about patterns. Developers take decisions in the code, and after processing them, they have a result. When they do that, they think about just the possible options they have from design.

Finding logic bugs and not just security bugs is not possible using static analysis or automated tools, and if testing is done from the application's design paradigms, the fails are never detected.

Options outside the chosen paradigms were not thought of. Attackers might.

* Try to understand how the application works using an HTTP proxy. Focus on the variables and parameters that could be used to control the application's flow.
* Launch automated tools for port scanning, vulnerability assessments, and configuration management issues.
* Replicate previous vulnerabilities between applications.

## Steps

1. Learn about the target application. The more you understand about the architecture and development process of a web application, the better you will be at spotting these vulnerabilities.
2. Intercept requests while browsing the site and pay attention to sensitive functionalities. Keep track of every request sent during these actions.
3. Use your creativity to think of ways to bypass access control or otherwise interfere with application logic.
4. Think of ways to combine the vulnerability you’ve found with other vulnerabilities to maximize the potential impact of the flaw.
5. Draft the report.

## Learn about the target

Start by learning about your target application. Browse the application as a regular user to uncover functionalities and interesting features. You can also read the application’s engineering blogs and documentation. The more you understand about the architecture, development process, and business needs of that application, the better you will be at spotting these vulnerabilities.

## Intercept requests while browsing

Intercept requests while browsing the site and pay attention to sensitive functionalities. Keep track of every request sent during these actions. Take note of how sensitive functionalities and access control are implemented, and how they interact with client requests.

## Think outside the box

Use your creativity to think of ways to bypass access control or otherwise interfere with application logic. Play with the requests that you have intercepted and craft requests that should not be granted.

## Escalation

Escalating application logic errors and broken access control depends entirely on the nature of the flaw you find. But a general rule of thumb is that you can try to combine the application logic error or broken access control with other vulnerabilities to increase their impact.

A broken access control that gives you access to the admin panel with a console or application deployment capabilities can lead to [remote code execution](rce.md). 

If you can find the configuration files of a web application, you can search for CVEs for the software versions in use to further compromise the application. 

You might also find credentials in a file that can be used to access different machines on the network.

Think of ways malicious users can exploit these vulnerabilities to the fullest extent, and communicate their impact in detail in the report.

## Portswigger lab writeups

* [Unprotected admin functionality](../acl/1.md)
* [Unprotected admin functionality with unpredictable URL](../acl/2.md)
* [User role controlled by request parameter](../acl/3.md)
* [User role can be modified in user profile](../acl/4.md)
* [User ID controlled by request parameter](../acl/5.md)
* [User ID controlled by request parameter, with unpredictable user IDs](../acl/6.md)
* [User ID controlled by request parameter with data leakage in redirect](../acl/7.md)
* [User ID controlled by request parameter with password disclosure](../acl/8.md)
* [Insecure direct object references](../acl/9.md)
* [URL-based access control can be circumvented](../acl/10.md)
* [Method-based access control can be circumvented](../acl/11.md)
* [Multistep process with no access control on one step](../acl/12.md)
* [Referer-based access control](../acl/13.md)

## Remediation

* Frameworks do not yet have the capability of automatically implementing permissions structures. Permissions structures need to be implemented by developers, because every application has specific, custom requirements. In most cases, the reason that access control is broken is because it has not been implemented. 
* When designing a permissions structure for an application, implement a "deny by default" (for all requests to all endpoints), and require allowlisting specific users/roles for any interaction to occur with that endpoint.

## Resources

* [Portswigger: Access control vulnerabilities and privilege escalation](https://portswigger.net/web-security/access-control)
* [OWASP: Broken Access Control](https://owasp.org/www-community/Broken_Access_Control)
* [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
* [Role and Attribute based Access Control for Node.js](https://www.npmjs.com/package/accesscontrol)
* [Bug Bounty Bootcamp](https://nostarch.com/bug-bounty-bootcamp)
* [Bug Bounty Hunting Essentials](https://www.packtpub.com/product/bug-bounty-hunting-essentials/9781788626897)
* [Bug Bounty Hunting for Web Security](https://link.springer.com/book/10.1007/978-1-4842-5391-5)

