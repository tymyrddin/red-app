| [![Portswigger Directory traversal Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#directory-traversal) |
|:--:|
| [Portswigger Academy Directory traversal Labs](https://portswigger.net/web-security/all-labs#directory-traversal) |

# Introduction

## What?

Directory traversal (also known as file path traversal) is a web security vulnerability that allows an attacker to read arbitrary files on the server that is running an application. This might include application code and data, credentials for back-end systems, and sensitive operating system files.

## Why?

In some cases, an attacker might be able to write to arbitrary files on the server, allowing them to modify application data or behaviour, and ultimately take full control of the server. 

## How?

* [File path traversal, simple case](1.md)
* [File path traversal, traversal sequences blocked with absolute path bypass](2.md)
* [File path traversal, traversal sequences stripped non-recursively](3.md)
* [File path traversal, traversal sequences stripped with superfluous URL-decode](4.md)
* [File path traversal, validation of start of path](5.md)
* [File path traversal, validation of file extension with null byte bypass](6.md)


