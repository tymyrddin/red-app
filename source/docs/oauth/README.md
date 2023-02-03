| [![Portswigger OAuth authentication Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#oauth-authentication) |
|:--:|
| [Portswigger Academy OAuth authentication Labs](https://portswigger.net/web-security/all-labs#oauth-authentication) |

# Introduction

## What?

The basic OAuth process is widely used to integrate third-party functionality that requires access to certain data from a user's account. For example, an application might use OAuth to request access to your email contacts list so that it can suggest people to connect with. However, the same mechanism is also used to provide third-party authentication services, allowing users to log in with an account that they have with a different website. 

## Why?

OAuth 2.0 is highly interesting for attackers because it is both extremely common and inherently prone to implementation mistakes. This can result in a number of vulnerabilities, allowing attackers to obtain sensitive user data and potentially bypass authentication completely. 

## How?

* [Single-sign-on security issues (SSO)](../techniques/sso.md)
* [Authentication bypass via OAuth implicit flow](1.md)
* [Forced OAuth profile linking](2.md)
* [OAuth account hijacking via redirect_uri](3.md)
* [Stealing OAuth access tokens via an open redirect](4.md)
* [SSRF via OpenID dynamic client registration](5.md)
* [Stealing OAuth access tokens via a proxy page](6.md)