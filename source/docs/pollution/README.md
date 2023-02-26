| [![Portswigger Prototype pollution Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#prototype-pollution) |
|:--:|
| [Portswigger Academy Prototype pollution Labs](https://portswigger.net/web-security/all-labs#prototype-pollution) |

# Introduction

## What?

Prototype pollution is a JavaScript vulnerability that enables an attacker to add arbitrary properties to global prototypes, which may then be inherited by user-defined objects. 

## Why?

Depending on the exact logic of the application, prototype pollution can lead to practically all popular web vulnerabilities: remote code execution (RCE), cross-site scripting ( XSS ), SQL injection, and so on.

## How?

* [Prototype pollution](../techniques/pollution.md)
* [DOM XSS via client-side prototype pollution](1.md)
* [DOM XSS via an alternative prototype pollution vector](2.md)
* [Client-side prototype pollution via flawed sanitization](3.md)
* [Client-side prototype pollution in third-party libraries](4.md)
* [Client-side prototype pollution via browser APIs](5.md)
* [Privilege escalation via server-side prototype pollution](6.md)
* [Detecting server-side prototype pollution without polluted property reflection](7.md)
* [Bypassing flawed input filters for server-side prototype pollution](8.md)
* [Remote code execution via server-side prototype pollution](9.md)
* [Exfiltrating sensitive data via server-side prototype pollution](10.md)
