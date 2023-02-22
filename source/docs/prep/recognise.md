# Target recognition

## Knockpy

[Knockpy](https://github.com/guelfoweb/knock) is a command-line tool for discovering subdomains. This is useful when you just have a main domain and you need to find all the applications or modules included in it. Verify the agreed scope of the assessment; you may not be allowed to look for all subdomains.

## HostileSubBruteforcer

[HostileSubBruteforcer](https://github.com/nahamsec/HostileSubBruteforcer) is a command-line tool that works very similar to Knockpy. It sends requests to a domain, using common names to create subdomains, in order to discover new ones and also servers that you did not know about. As with Knockpy, this is active, so check agreed scope.

## FFuf

[FFUF](https://github.com/ffuf/ffuf), short for “Fuzz Faster you Fool” is an open-source web fuzzing tool written in Go programming language, intended for discovering elements and content within web applications, or web servers.

* General Directory discovery with the option to fuzz at any place in the URL.
* VHOST discovery without DNS Records
* Fuzzing using various HTTP methods.

## Assetfinder

[assetfinder](https://github.com/tomnomnom/assetfinder) is a Go-based tool to find related domains and subdomains that are potentially related to a given domain from a variety of sources, including those in the infosec space and social networks which can give relevant info:

* crt.sh
* certspotter
* hackertarget
* threatcrowd
* wayback machine
* dns.bufferover.run
* facebook – Needs [FB_APP_ID and FB_APP_SECRET environment variables set](https://developers.facebook.com/) and you need to be careful with your app’s rate limits
* virustotal – Needs [VT_API_KEY environment variable set](https://developers.virustotal.com/reference)
* findsubdomains – Needs [SPYSE_API_TOKEN environment variable set](https://spyse.com/apidocs) (the free version always gives the first response page, and you also get “25 unlimited requests”)

## Nmap

[Nmap](https://nmap.org/), without doubt, is the most powerful port scanner and service
enumerator that exists and has vulnerability scanning capabilities.

## Rustscan

[RustScan](https://github.com/RustScan/RustScan) is a modern take on port scanning. It uses Adaptive Learning to improve itself over time. It scans so fast that I recommend 

## Shodan

[Shodan](https://www.shodan.io) is the largest IT database in the world. It includes information about hosts, technologies supported, domain changes, information tracked by searchers, and sensitive information, such as configuration files, IP addresses, and credentials. You can access Shodan through an API, but you need to pay for full access.

## What CMS

[What CMS](https://whatcms.org/) is an online tool that helps to determine if a website is using a CMS, and if it does, it identifies what CMS the website is using.

## Recon-ng

[Recon-ng](https://bitbucket.org/LaNMaSteR53/recon-ng) is a Reconnaissance framework that uses different sources to ask for information about a target. The results include name servers, IP addresses, subdomains, and inclusive zone transfers when these are allowed to be consulted.