| [![Portswigger Prototype pollution Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#prototype-pollution) |
|:--:|
| [Portswigger Academy Prototype pollution Labs](https://portswigger.net/web-security/all-labs#prototype-pollution) |

# Introduction

## What?

Prototype pollution is a JavaScript vulnerability that enables an attacker to add arbitrary properties to global prototypes, which may then be inherited by user-defined objects. 

## Why?

Depending on the exact logic of the application, prototype pollution can lead to practically all popular web vulnerabilities: remote code execution (RCE), cross-site scripting ( XSS ), SQL injection, and so on.

## How?

* [DOM XSS via client-side prototype pollution](1.md)
* [DOM XSS via an alternative prototype pollution vector](2.md)
* [Client-side prototype pollution in third-party libraries](3.md)
* [Client-side prototype pollution via browser APIs](4.md)
* [Client-side prototype pollution via flawed sanitization](5.md)



