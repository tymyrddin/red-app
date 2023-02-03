| [![Portswigger Information disclosure Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#information-disclosure) |
|:--:|
| [Portswigger Academy Information disclosure Labs](https://portswigger.net/web-security/all-labs#information-disclosure) |

# Introduction

## What?

Information disclosure, also known as information leakage, is when a website unintentionally reveals sensitive information to its users. Depending on the context, websites may leak all kinds of information to a potential attacker, including: Data about other users, such as usernames or financial information, Sensitive commercial or business data, and technical details about the website and its infrastructure.

## Why?

The dangers of leaking sensitive user or business data are fairly obvious, but disclosing technical information can sometimes be just as serious. Although some of this information will be of limited use, it can potentially be a starting point for exposing an additional attack surface, which may contain other interesting vulnerabilities. The knowledge that you are able to gather could even provide the missing piece of the puzzle when trying to construct complex, high-severity attacks.

Occasionally, sensitive information might be carelessly leaked to users who are simply browsing the website in a normal fashion. More commonly, however, an attacker needs to elicit the information disclosure by interacting with the website in unexpected or malicious ways. They will then carefully study the website's responses to try and identify interesting behavior. 

## How?

* [Information disclosure techniques](../techniques/disclosure.md)
* [Information disclosure in error messages](1.md)
* [Information disclosure on debug page](2.md)
* [Source code disclosure via backup files](3.md)
* [Authentication bypass via information disclosure](4.md)
* [Information disclosure in version control history](5.md)