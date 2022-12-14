# Single-sign-on security issues (SSO)

Cookie sharing, SAML, and OAuth are the three most common ways of implementing SSO. Each mechanism has unique strengths and weaknesses, and developers choose different approaches depending on their needs.

## Steps

1. If the target application is using single sign-on, determine the SSO mechanism in use.
2. If the application is using shared session cookies, try to steal session cookies by using subdomain takeovers.
3. If the application uses a SAML-based SSO scheme, test whether the server is verifying SAML signatures properly.
4. If the application uses OAuth, try to steal OAuth tokens by using open redirects.
5. Create report.

## Subdomain takeovers

1. List the target's subdomains.
2. Find unregistered pages.
3. Register the page.

## SAML vulnerabilities

1. Locate the `SAML` response
2. Analyse the response fields
3. Bypass the signature
4. Re-encode the message

## OAuth token theft

1. Determine whether the website is using `OAuth`.
2. Look for [open redirect vulnerabilities](redirects.md).
3. Try to exfiltrate the OAuth tokens by using one of the open redirects found.

## Escalation

SSO bypass usually means that attackers can take over the accounts of others. Therefore, these vulnerabilities are of high severity before any escalation attempts. 

You can escalate SSO bypass vulnerabilities by attempting to take over accounts with high privileges, such as `admin` accounts.

After you have taken over a user's account on one site, try to access the victim's account on other sites by using the same OAuth credentials.

You can escalate account takeovers by writing a script to automate the takeover of large numbers of accounts. 

And, you can try to leak data, execute sensitive actions, or take over the application by using the accounts taken over.

## Resources

* [Daily Swig: Vulnerabilities in Single Sign-On services could be abused to bypass authentication controls](https://portswigger.net/daily-swig/vulnerabilities-in-single-sign-on-services-could-be-abused-to-bypass-authentication-controls)
* [Bug Bounty Bootcamp](https://nostarch.com/bug-bounty-bootcamp)
* [Bug Bounty Hunting Essentials](https://www.packtpub.com/product/bug-bounty-hunting-essentials/9781788626897)
* [Bug Bounty Hunting for Web Security](https://link.springer.com/book/10.1007/978-1-4842-5391-5)


