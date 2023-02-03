| [![Portswigger OS command injection Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#os-command-injection) |
|:--:|
| [Portswigger Academy OS command injection Labs](https://portswigger.net/web-security/all-labs#os-command-injection) |

# Introduction

## What?

OS command injection (also known as shell injection) is a web security vulnerability that allows an attacker to execute arbitrary operating system (OS) commands on the server that is running an application, and typically fully compromise the application and all its data.

## Why?

Very often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure, exploiting trust relationships to pivot the attack to other systems within the organization. 

## How?

* [Remote code execution (RCE)](../techniques/rce.md)
* [OS command injection, simple case](1.md)
* [Blind OS command injection with time delays](2.md)
* [Blind OS command injection with output redirection](3.md)
* [Blind OS command injection with out-of-band interaction](4.md)
* [Blind OS command injection with out-of-band data exfiltration](5.md)


