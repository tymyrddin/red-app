# HTTP Request smuggling techniques

HTTP request smuggling vulnerabilities occur when the frontend and the backend interpret the boundary of an HTTP request differently causing de-synchronisation between them. This is due to numerous frontend and backend libraries deviating from RFC specifications when dealing with both the `Content-Length` and the `Transfer-Encoding` header. HTTP request bodies can be framed according to these two headers and deviations from the specification occur. As a result, part of a request gets appended or smuggled, to the next one which allows the response of the smuggled request to be provided to another user.

* The body can vary from application to application, framework to framework, and the `Content-Length` and `Transfer-Encoding: chunked` headers are applicable to `HTTP/1.1` and partially to `HTTP/1.0`, but **not** to `HTTP/2`.
* HTTP 1.1 allows for sending both `Content-Length` (`CL`) and `Transfer-Encoding` (`TE`) headers in the same request, but when both are sent, `TE` takes precedence.
* A **Parent Smuggled Request** is a request that has both the headers to trick the servers.
* A **Child Smuggled Request** is an ideal request hidden inside the parent-smuggled request. It is called ideal because it only has one of the headers.

## Steps

Send malformed requests to check for:

* `CL:CL`: When provided with two `Content-Length` headers, if implementation differences occur between a frontend and a backend on which `Content-Length` header is prioritised, smuggling attacks can occur.
* `CL:TE`: Different HTTP libraries tolerate different variations of the `Transfer-Encoding` header and will normalise them to improve client experience. By understanding what variations of the `TE` header is normalised by the backend server, it might be possible to smuggle a malformed `TE` header through the frontend and conduct a `CL:TE` smuggling attack. The first part of a request declares a short chunk length, typically `0`. The frontend server reads only the first part of the request and passes the second part to the back-end server.
* `TE:TE`: Frontend and backend servers correctly prioritise the Transfer-Encoding header, but the header can be obfuscated to trick one of the servers.
* `TE:CL` : The frontend server prioritises the `Transfer-Encoding` weakness, while the backend server prioritises the `Content-Length` weakness, making it possible to declare the length of the first chunk up to and including the malicious request. The second chunk is declared as having `0` length, so the frontend server assumes the request is complete. It passes the request to the backend server, which receives and processes it.

## Impact

When an attacker succeeds in performing a request smuggling attack,it can allow the attacker to:

* Gain access to protected resources, such as admin consoles
* Gain access to sensitive data
* Hijack sessions of web users
* Launch cross-site scripting (XSS) attacks without requiring any action from the user
* Perform credential hijacking

## Remediation

The responsibility of remediation falls onto a backend maintainer as much as a frontend maintainer. One can argue that normalising malformed headers should be acceptable behaviour from a backend and make it more tolerant to user faults, and that the real problem is proxies which forward these requests without normalising them first. But due to the vast ecosystem of dependencies and numerous libraries fitting the criteria of a backend and frontend, both parties are to try and remediate these issues.

* Interpret HTTP headers consistently on front-end and back-end servers. This is not always an option, as load balancers are generally hardware appliances that support backend servers, which are run on distinct platforms. If you cannot run the same software on both front-end and back-end, at least be aware of how each server deals with HTTP headers, and ensure they consistently interpret HTTP requests.
* If possible, disable connection reuse on the backend server. This can completely prevent HTTP request smuggling.
* Disable vulnerable optimisations: If it is not possible to change backend configurations, disable any performance optimisations that use the `Transfer-Encoding` or `Content-Length` header. 
* If possible, avoid the use of load balancers, content delivery networks (CDNs), or reverse proxies.
* Use HTTP/2, making sure that frontend and backend servers only communicate using the HTTP/2 protocol. This is no guarantee, but makes the attacks harder to perform.
* Configure the frontend server to normalise ambiguous requests to prevent malicious requests being passed to the backend server.
* HTTP logs should only be available to administrative users, to avoid exposing unintended parts of an HTTP request to potential attackers.
* Use a web application firewall (WAF) which can identify and blocks or sanitise HTTP traffic. Check whether any changes to the WAF configuration are required to safeguard against an HTTP request smuggling vulnerability.
