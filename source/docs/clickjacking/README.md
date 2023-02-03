| [![Portswigger Clickjacking Labs](../../_static/images/pal.png)](https://portswigger.net/web-security/all-labs#clickjacking) |
|:--:|
| [Portswigger Academy Clickjacking Labs](https://portswigger.net/web-security/all-labs#clickjacking) |

# Introduction

## What?

Clickjacking attacks rely on visual tricks to get website visitors to click on user interface elements that will perform actions on another website. The attack is performed by hiding the target website's UI and arranging the visible UI so that the user is not aware of clicking the target website. Due to this UI arrangement, this kind of attack is also known as UI redressing or UI redress attack.

## Why?

The goal of a clickjacking attack is to trick unsuspecting website visitors into actions on another website like transferring money, purchasing products, downloading malware, give them like on a social network, and so on. 

## How?

* [Clickjacking techniques](../techniques/clickjacking.md)
* [Basic clickjacking with CSRF token protection](1.md)
* [Clickjacking with form input data prefilled from a URL parameter](2.md)
* [Clickjacking with a frame buster script](3.md)
* [Exploiting clickjacking vulnerability to trigger DOM-based XSS](4.md)
* [Multistep clickjacking](5.md)