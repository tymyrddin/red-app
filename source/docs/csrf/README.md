| [![Portswigger CSRF Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#cross-site-request-forgery-csrf) |
|:--:|
| [Portswigger Academy CSRF Labs](https://portswigger.net/web-security/all-labs#cross-site-request-forgery-csrf) |

# Introduction

## What?

Cross-site request forgery (CSRF) is a client-side technique used to attack other users of a web application. 

## Why?

Cross-site request forgery is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.

Using CSRF, attackers can send HTTP requests that pretend to come from the victim, carrying out unwanted actions on a victim’s behalf. For example, an attacker could change a password or transfer money from a bank account without permission. If the victim is an administrative account, CSRF can compromise the entire web application.

## How?

* [Cross-site request forgery (CSRF) techniques](../techniques/csrf.md)
* [CSRF vulnerability with no defenses](1.md)
* [CSRF where token validation depends on request method](2.md)
* [CSRF where token validation depends on token being present](3.md)
* [CSRF where token is not tied to user session](4.md)
* [CSRF where token is tied to non-session cookie](5.md)
* [CSRF where token is duplicated in cookie](6.md)
* [SameSite Lax bypass via method override](7.md)
* [SameSite Strict bypass via client-side redirect](8.md)
* [SameSite Strict bypass via sibling domain](9.md)
* [SameSite Lax bypass via cookie refresh](10.md)
* [CSRF where Referer validation depends on header being present](11.md)
* [CSRF with broken Referer validation](12.md)
