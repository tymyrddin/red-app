| [![Portswigger Server-side request forgery Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#server-side-request-forgery-ssrf) |
|:--:|
| [Portswigger Academy Server-side request forgery Labs](https://portswigger.net/web-security/all-labs#server-side-request-forgery-ssrf) |

# Introduction

## What?

Server-side request forgery (also known as SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make requests to an unintended location. 

## Why?

In a typical SSRF attack, the attacker might cause the server to make a connection to internal-only services within the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems, potentially leaking sensitive data such as authorization credentials. 

## How?

* [Server-side request forgery (SSRF) techniques](../techniques/ssrf.md)
* [Basic SSRF against the local server](1.md)
* [Basic SSRF against another back-end system](2.md)
* [SSRF with blacklist-based input filter](3.md)
* [SSRF with filter bypass via open redirection vulnerability](4.md)
* [Blind SSRF with out-of-band detection](5.md)
* [SSRF with whitelist-based input filter](6.md)
* [Blind SSRF with Shellshock exploitation](7.md)


