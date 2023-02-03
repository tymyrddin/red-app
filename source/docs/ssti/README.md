| [![Portswigger Server-side template injection Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#server-side-template-injection) |
|:--:|
| [Portswigger Academy Server-side template injection Labs](https://portswigger.net/web-security/all-labs#server-side-template-injection) |

# Introduction

## What?

Server-side template injection is when an attacker is able to use native template syntax to inject a malicious payload into a template, which is then executed server-side.

## Why?

Template engines are designed to generate web pages by combining fixed templates with volatile data. Server-side template injection attacks can occur when user input is concatenated directly into a template, rather than passed in as data. This allows attackers to inject arbitrary template directives in order to manipulate the template engine, often enabling them to take complete control of the server. As the name suggests, server-side template injection payloads are delivered and evaluated server-side, potentially making them much more dangerous than a typical client-side template injection. 

## How?

* [Template injection (SSTI) techniques](../techniques/ssti.md)
* [Basic server-side template injection](1.md)
* [Basic server-side template injection (code context)](2.md)
* [Server-side template injection using documentation](3.md)
* [Server-side template injection in an unknown language with a documented exploit](4.md)
* [Server-side template injection with information disclosure via user-supplied objects](5.md)
* [Server-side template injection in a sandboxed environment](6.md)
* [Server-side template injection with a custom exploit](7.md)


