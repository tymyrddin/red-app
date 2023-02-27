| [![Portswigger Academy JWT Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#jwt) |
|:--:|
| [Portswigger Academy JWT Labs](https://portswigger.net/web-security/all-labs#jwt) |

# Introduction

## What?

JSON web tokens (JWTs) are a standardised format for sending cryptographically signed JSON data between systems. They can theoretically contain any kind of data, but are most commonly used to send information ("claims") about users as part of authentication, session handling, and access control mechanisms.

JWTs are a popular choice for highly distributed websites where users need to interact seamlessly with multiple back-end servers. 

## Why?

Design issues and flawed handling of JSON web tokens (JWTs) can leave websites vulnerable to a variety of high-severity attacks. As JWTs are most commonly used in authentication, session management, and access control mechanisms, these vulnerabilities can potentially compromise the entire website and its users. 

## How?

* [JSON web tokens attacks](../techniques/jwt.md)
* [JWT authentication bypass via unverified signature](1.md)
* [JWT authentication bypass via flawed signature verification](2.md)
* [JWT authentication bypass via weak signing key](3.md)
* [JWT authentication bypass via jwk header injection](4.md)
* [JWT authentication bypass via jku header injection](5.md)
* [JWT authentication bypass via kid header path traversal](6.md)
* [JWT authentication bypass via algorithm confusion](7.md)
* [JWT authentication bypass via algorithm confusion with no exposed key](8.md)
