| [![Portswigger HTTP request smuggling Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#http-request-smuggling) |
|:--:|
| [Portswigger Academy HTTP request smuggling Labs](https://portswigger.net/web-security/all-labs#http-request-smuggling) |

# Introduction

## What?

HTTP request smuggling is a technique for interfering with the way a web site processes sequences of HTTP requests that are received from one or more users.

## Why?

Request smuggling vulnerabilities are often critical in nature, and can be exploited to bypass security controls, gain unauthorised access to sensitive data, directly compromise other application users, to conduct phishing attacks, cache poisoning, cross-site scripting (XSS), and more.  More information regarding exploiting this vulnerability was published by James Kettle during BlackHAT USA 2019, titled [HTTP Desync Attacks: Request Smuggling Reborn](https://portswigger.net/research/http-desync-attacks-request-smuggling-reborn).

## How?

* [HTTP Request smuggling techniques](smuggling.md)
* [HTTP request smuggling, basic CL.TE vulnerability](1.md)
* [HTTP request smuggling, basic TE.CL vulnerability](2.md)
* [HTTP request smuggling, obfuscating the TE header](3.md)
* [HTTP request smuggling, confirming a CL.TE vulnerability via differential responses](4.md)
* [HTTP request smuggling, confirming a TE.CL vulnerability via differential responses](5.md)
* [Exploiting HTTP request smuggling to bypass front-end security controls, CL.TE vulnerability](6.md)
* [Exploiting HTTP request smuggling to bypass front-end security controls, TE.CL vulnerability](7.md)
* [Exploiting HTTP request smuggling to reveal front-end request rewriting](8.md)
* [Exploiting HTTP request smuggling to capture other users’ requests](9.md)
* [Exploiting HTTP request smuggling to deliver reflected XSS](10.md)
* [Response queue poisoning via H2.TE request smuggling](11.md)
* [H2.CL request smuggling](12.md)
* [HTTP/2 request smuggling via CRLF injection](13.md)
* [HTTP/2 request splitting via CRLF injection](14.md)
* [CL.0 request smuggling](15.md)
* [Exploiting HTTP request smuggling to perform web cache poisoning](16.md)
* [Exploiting HTTP request smuggling to perform web cache deception](17.md)
* [Bypassing access controls via HTTP/2 request tunnelling](18.md)
* [Web cache poisoning via HTTP/2 request tunnelling](19.md)
* [Client-side desync](20.md)
* [Browser cache poisoning via client-side desync](21.md)
* [Server-side pause-based request smuggling](22.md)