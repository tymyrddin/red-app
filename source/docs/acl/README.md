| [![Portswigger Access control vulnerabilities Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#access-control-vulnerabilities) |
|:--:|
| [Portswigger Academy Access control vulnerabilities Labs](https://portswigger.net/web-security/all-labs#access-control-vulnerabilities) |

# Introduction

## What?

Access control (or authorisation) is the application of constraints on who (or what) can perform attempted actions or access resources that they have requested. In the context of web applications, access control is dependent on authentication and session management:

* Authentication identifies the user and confirms that they are who they say they are.
* Session management identifies which subsequent HTTP requests are being made by that same user.
* Access control determines whether the user is allowed to carry out the action that they are attempting to perform.

## Why?

Broken access controls are a commonly encountered and often critical security vulnerability. Design and management of access controls is a complex and dynamic problem that applies business, organisational, and legal constraints to a technical implementation. Access control design decisions have to be made by humans, not technology, and the potential for errors is high. 

## How?

* [Application logic errors and broken access control vulnerabilities](../techniques/access.md)
* [Insecure direct object references (IDOR)](../techniques/idor.md)
* [Unprotected admin functionality](1.md)
* [Unprotected admin functionality with unpredictable URL](2.md)
* [User role controlled by request parameter](3.md)
* [User role can be modified in user profile](4.md)
* [User ID controlled by request parameter](5.md)
* [User ID controlled by request parameter, with unpredictable user IDs](6.md)
* [User ID controlled by request parameter with data leakage in redirect](7.md)
* [User ID controlled by request parameter with password disclosure](8.md)
* [Insecure direct object references](9.md)
* [URL-based access control can be circumvented](10.md)
* [Method-based access control can be circumvented](11.md)
* [Multistep process with no access control on one step](12.md)
* [Referer-based access control](13.md)


