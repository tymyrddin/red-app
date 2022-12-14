# Server-side request forgery (SSRF)

A server-side request forgery (SSRF) attack involves forcing some server-side application to make HTTP requests to a 
domain of our choosing. This can sometimes grant access to internal resources or unprotected admin panels.

## Steps

1. Spot the features prone to SSRFs and take notes for future reference.
2. Set up a callback listener to detect blind SSRFs by using an online service, Netcat, or Burp’s Collaborator feature.
3. Provide the potentially vulnerable endpoints with common internal addresses or the address of your callback listener.
4. Check if the server responds with information that confirms the SSRF. Or, in the case of a blind SSRF, check your server logs for requests from the target server.
5. In the case of a blind SSRF, check if the server behaviour differs when you request different hosts or ports.
6. If SSRF protection is implemented, try to bypass it by using the strategies discussed in this chapter.
7. Pick a tactic to escalate the SSRF.
8. Draft report.

## Spot features prone to SSRFs

SSRFs occur in features that require visiting and fetching external resources. These include webhooks, file uploads, document and image processors, link expansions or thumbnails, and proxy services. It is also worth testing any endpoint that processes a user-provided URL. And pay attention to potential SSRF entry points that are less obvious, like URLs embedded in files that are processed by the application (XML files and PDF files can often be used to trigger SSRFs), hidden API endpoints that accept URLs as input, and input that gets inserted into HTML tags.

## Provide potentially vulnerable endpoints with internal URLs

Once you’ve identified the potentially vulnerable endpoints, provide internal addresses as the URL inputs to these endpoints. Depending on the network configuration, you might need to try several addresses before you find the ones in use by the network.

## Check the results

In the case of regular SSRF, see if the server returns a response that reveals any information about the internal service. For example, does the response contain service banners or the content of internal pages?

The easiest way of detecting blind SSRFs is through out-of-band techniques: you make the target send requests to an external server that you control, and then monitor your server logs for requests from the target.

Being able to generate an outbound request from the target server alone is not an exploitable issue. Since you cannot use blind SSRFs to read internal files or access internal services, you need to confirm their exploitability by trying to explore the internal network with the SSRF. Make requests to various target ports and see if server behaviour differs between commonly open and closed ports.

## Bypassing protections

What if you submit an SSRF payload, but the server returns this response?

    Error. Requests to this address are not allowed. Please try again.

This SSRF was blocked by a protection mechanism, possibly a URL allowlist or blocklist. The site may have protection mechanisms implemented, but this does not mean that the protection is complete.

Allowlists are generally the hardest to bypass, because they are, by default, stricter than blocklists. But getting around them is still possible if you can find an open redirect vulnerability within the allowlisted domains. If you find one, you can request an allowlisted URL that redirects to an internal URL.

Since applications often need to fetch resources from a variety of internet sources, most SSRF protection mechanisms come in the form of a blocklist. If you’re faced with a blocklist, there are many ways of tricking the server.

* Fooling it with redirects
* Using IPv6 addresses
* Tricking the server with DNS
* Switching out the encoding

## Escalation

What you can achieve with an SSRF usually depends on the internal services found on the network. Depending on the situation, you could use SSRF to scan the network for reachable hosts, port-scan internal machines to fingerprint internal services, collect instance metadata, bypass access controls, exfiltrate confidential data, and even execute code on reachable machines.

### Scan the network

Reachable machines are other network hosts that can be connected to via the current machine. These internal machines might host databases, internal websites, and otherwise sensitive functionalities that can be exploited.

### Pulling AWS instance metadata

Amazon Elastic Compute Cloud (EC2), offers an instance metadata tool that enables EC2 instances to access data about themselves by querying the API endpoint at `169.254.169.254`. 

    http://169.254.169.254/latest/meta-data/

Use this URL in an endpoint vulnerable to SSRF:

    https://public.example.com/proxy?url=http://169.254.169.254/latest/meta-data/

These API endpoints are accessible by default unless network admins specifically block or disable them. The information these services reveal is often extremely sensitive and could allow attackers to escalate SSRFs to serious information leaks and even [RCE](rce.md).

### Google Cloud metadata

If the company uses Google Cloud, query the Google Instance Metadata API instead. Google implements additional security measures for its API endpoints, so querying Google Cloud Metadata APIv1 requires one of these special headers:

    Metadata-Flavor: Google
    X-Google-Metadata-Request: True

These headers offer protection against SSRFs because most often during an SSRF, you cannot specify special headers for the forged request. We will need to find a way to forge the required headers to request instance metadata for targets that use Google Cloud.

### Exploiting blind SSRFs

Because blind SSRFs don’t return a response or error message, their exploitation is often limited to network mapping, port scanning, and service discovery. And because you can’t extract information directly from the target server, this exploitation relies heavily on inference. Yet by analyzing HTTP status codes and server response times, we can often achieve results similar to regular SSRF.

Use what you’ve found by scanning the network, identifying services, and pulling instance metadata to execute attacks that have impact. Notably, you may be able to bypass access controls, leak confidential information, and execute code.

## Resources

* [Portswigger: Server-side request forgery (SSRF)](https://portswigger.net/web-security/ssrf)
* [OWASP: Testing for Server-Side Request Forgery](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery)
* [Instance metadata categories](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-categories.html)
* [EC2 API endpoints](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)
* [Fetch Metadata Request Headers](https://www.w3.org/TR/fetch-metadata/)
* [Google Cloud: About VM metadata](https://cloud.google.com/compute/docs/metadata/overview)
* [Oracle Cloud: Getting Instance Metadata](https://docs.oracle.com/en-us/iaas/Content/Compute/Tasks/gettingmetadata.htm)
* [Bug Bounty Bootcamp](https://nostarch.com/bug-bounty-bootcamp)
* [Bug Bounty Hunting Essentials](https://www.packtpub.com/product/bug-bounty-hunting-essentials/9781788626897)
* [Bug Bounty Hunting for Web Security](https://link.springer.com/book/10.1007/978-1-4842-5391-5)
