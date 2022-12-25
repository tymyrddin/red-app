# Re-useful extensions

## FoxyProxy

In some assessments/pentests, it is important to check applications from different geolocalisations,
and it is possible to configure a public or private VPN in [FoxyProxy](https://addons.mozilla.org/es/firefox/addon/foxyproxy-standard/). 

## User-Agent Switcher

Some applications work differently depending on web browser and devices from which they are opened. It is not possible to have all browsers, versions, and devices to test them. But, all of them are identified by a parameter included in the HTTP request, the `User-Agent`. 

It can be switched directly in the HTTP request using an HTTP proxy, but to make lives easier, use the [User-Agent Switcher](https://addons.mozilla.org/es/firefox/addon/user-agent-switcher/), a Firefox add-on that has a lot of user agents to analyse different responses.

## HackBar

[HackBar](https://addons.mozilla.org/es/firefox/addon/hackbar/) is an add-on that integrates a simple HTTP proxy into Firefox, providing the capability to manipulate HTTP requests. Only use  for simple testing.

## Cookies Manager+

[Cookies Manager+](https://addons.mozilla.org/es/firefox/addon/cookies-manager-plus/) allows creating and modifying cookies so they can be read by the application and change the behaviour. It is also useful for checking the authorisation and authentication controls, where the user sessions and privileges are stored in cookies.