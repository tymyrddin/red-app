| [![Portswigger Cross-site scripting Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#cross-site-scripting) |
|:--:|
| [Portswigger Academy Cross-site scripting Labs](https://portswigger.net/web-security/all-labs#cross-site-scripting) |

# Introduction

## What?

XSS is so prevalent that, year after year, it shows up in OWASP’s list of the top 10 vulnerabilities threatening web applications.

## Why?

An XSS vulnerability occurs when attackers can execute custom scripts on a victim’s browser. If an application fails to distinguish between user input and the legitimate code that makes up a web page, attackers can inject their own code into pages viewed by other users. The victim’s browser will then execute the malicious script, which might steal cookies, leak personal information, change site contents, or redirect the user to a malicious site. These malicious scripts are often JavaScript code but can also be HTML, Flash, VBScript, or anything written in a language that the browser can execute.

## How?

* [Cross-site scripting (XSS) techniques](../techniques/xss.md)
* [Open redirection techniques](../techniques/redirects.md)
* [Reflected XSS into HTML context with nothing encoded](1.md)
* [Stored XSS into HTML context with nothing encoded](2.md)
* [DOM XSS in document.write sink using source location.search](3.md)
* [DOM XSS in innerHTML sink using source location.search](4.md)
* [DOM XSS in jQuery anchor href attribute sink using location.search source](5.md)
* [DOM XSS in jQuery selector sink using a hashchange event](6.md)
* [Reflected XSS into attribute with angle brackets HTML-encoded](7.md)
* [Stored XSS into anchor href attribute with double quotes HTML-encoded](8.md)
* [Reflected XSS into a JavaScript string with angle brackets HTML encoded](9.md)
* [DOM XSS in document.write sink using source location.search inside a select element](10.md)
* [DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded](11.md)
* [Reflected DOM XSS](12.md)
* [Stored DOM XSS](13.md)
* [Exploiting cross-site scripting to steal cookies](14.md)
* [Exploiting cross-site scripting to capture passwords](15.md)
* [Exploiting XSS to perform CSRF](16.md)
* [Reflected XSS into HTML context with most tags and attributes blocked](17.md)
* [Reflected XSS into HTML context with all tags blocked except custom ones](18.md)
* [Reflected XSS with some SVG markup allowed](19.md)
* [Reflected XSS in canonical link tag](20.md)
* [Reflected XSS into a JavaScript string with single quote and backslash escaped](21.md)
* [Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped](22.md)
* [Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped](23.md)
* [Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped](24.md)
* [Reflected XSS with event handlers and href attributes blocked](25.md)
* [Reflected XSS in a JavaScript URL with some characters blocked](26.md)
* [Reflected XSS with AngularJS sandbox escape without strings](27.md)
* [Reflected XSS with AngularJS sandbox escape and CSP](28.md)
* [Reflected XSS protected by very strict CSP, with dangling markup attack](29.md)
* [Reflected XSS protected by CSP, with CSP bypass](30.md)