| [![Portswigger HTTP Host header attacks Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#http-host-header-attacks) |
|:--:|
| [Portswigger Academy HTTP Host header attacks Labs](https://portswigger.net/web-security/all-labs#http-host-header-attacks) |

# Introduction

## What?

The purpose of the HTTP Host header is to help identify which back-end component the client wants to communicate with. If requests didn't contain Host headers, or if the Host header was malformed in some way, this could lead to issues when routing incoming requests to the intended application.

Historically, this ambiguity didn't exist because each IP address would only host content for a single domain. Nowadays, largely due to the ever-growing trend for cloud-based solutions and outsourcing much of the related architecture, it is common for multiple websites and applications to be accessible at the same IP address. This approach has also increased in popularity partly as a result of IPv4 address exhaustion. 

## Why?

HTTP Host header attacks exploit vulnerable websites that handle the value of the Host header in an unsafe way. If the server implicitly trusts the Host header, and fails to validate or escape it properly, an attacker may be able to use this input to inject harmful payloads that manipulate server-side behavior. Attacks that involve injecting a payload directly into the Host header are often known as "Host header injection" attacks.

The header value may also be used in a variety of interactions between different systems of the website's infrastructure.

As the Host header is in fact user controllable, this practice can lead to a number of issues. If the input is not properly escaped or validated, the Host header is a potential vector for exploiting a range of other vulnerabilities, most notably: Web cache poisoning, Business logic flaws in specific functionality, Routing-based SSRF, and Classic server-side vulnerabilities, such as SQL injection

## How?

* [HTTP Host header attacks](../techniques/headers.md)
* [Basic password reset poisoning](1.md)
* [Host header authentication bypass](2.md)
* [Web cache poisoning via ambiguous requests](3.md)
* [Routing-based SSRF](4.md)
* [SSRF via flawed request parsing](5.md)
* [Host validation bypass via connection state attack](6.md)
* [Password reset poisoning via dangling markup](7.md)