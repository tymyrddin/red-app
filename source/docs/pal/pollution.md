# Prototype pollution

## DOM XSS via client-side prototype pollution

### Description

[This lab](https://portswigger.net/web-security/prototype-pollution/finding/lab-prototype-pollution-dom-xss-via-client-side-prototype-pollution) is vulnerable to DOM XSS via client-side prototype pollution.

### Proof of concept

#### Find a prototype pollution source

1. In your browser, try polluting the `Object.prototype` by injecting an arbitrary property via the query string:

```text
/?__proto__[foo]=bar
```

2. Open the browser DevTools panel and go to the Console tab.
3. Enter `Object.prototype`.
4. Study the properties of the returned object. Observe that it now has a `foo` property with the value bar. You've successfully found a prototype pollution source.

#### Identify a gadget

1. In the browser DevTools panel, go to the Sources tab.
2. Study the JavaScript files that are loaded by the target site and look for any DOM XSS sinks.
3. In `searchLogger.js`, notice that if the config object has a `transport_url` property, this is used to dynamically append a script to the DOM.
4. Notice that no `transport_url` property is defined for the `config` object. This is a potential gadget for controlling the `src` of the `script` element.

#### Craft an exploit

1. Using the prototype pollution source you identified earlier, try injecting an arbitrary `transport_url` property:

```text
/?__proto__[transport_url]=foo
```

2. In the browser DevTools panel, go to the Elements tab and study the HTML content of the page. Observe that a `script` element has been rendered on the page, with the `src` attribute `foo`.
3. Modify the payload in the URL to inject an XSS proof-of-concept. For example, you can use a `data: URL`:

```text
/?__proto__[transport_url]=data:,alert(1);
```

4. Observe that the `alert(1)` is called and the lab is solved.

### Exploitability

An attacker will need to find a source that can be used to add arbitrary properties to the global `Object.prototype`; identify a gadget property that allows for executing arbitrary JavaScript; combine these to call `alert()`.

This lab can be solved manually in a browser, or by using [DOM Invader](https://portswigger.net/burp/documentation/desktop/tools/dom-invader). 

----

## DOM XSS via an alternative prototype pollution vector

### Description

[This lab](https://portswigger.net/web-security/prototype-pollution/finding/lab-prototype-pollution-dom-xss-via-an-alternative-prototype-pollution-vector) is vulnerable to DOM XSS via client-side prototype pollution.

### Proof of concept

1. Load the lab in Burp's built-in browser.
2. Enable [DOM Invader](https://portswigger.net/burp/documentation/desktop/tools/dom-invader) and enable the prototype pollution option.
3. Open the browser DevTools panel and go to the **DOM Invader** tab and reload the page.
4. Observe that DOM Invader has identified a prototype pollution vector in the search property i.e. the query string.
5. Click **Scan for gadgets**. A new tab opens in which DOM Invader begins scanning for gadgets using the selected source.
6. When the scan is complete, open the DevTools panel in the same tab as the scan, then go to the **DOM Invader** tab.
7. Observe that DOM Invader has successfully accessed the `eval()` sink via the sequence gadget.
8. Click Exploit. Observe that DOM Invader's auto-generated proof-of-concept doesn't trigger an `alert()`.
9. Go back to the previous browser tab and look at the `eval()` sink again in **DOM Invader**. Notice that following the closing canary string, a numeric 1 character has been appended to the payload.
10. Click Exploit again. In the new tab that loads, append a minus character (`-`) to the URL and reload the page.
11. Observe that the `alert(1)` is called and the lab is solved.

### Exploitability

An attacker will need to find a source that can be used to add arbitrary properties to the global `Object.prototype`; identify a gadget property that allows for executing arbitrary JavaScript; combine these to call `alert()`.

This lab can be solved manually in a browser, or by using [DOM Invader](https://portswigger.net/burp/documentation/desktop/tools/dom-invader). 

----

## Client-side prototype pollution in third-party libraries

### Description

[This lab](https://portswigger.net/web-security/prototype-pollution/finding/lab-prototype-pollution-client-side-prototype-pollution-in-third-party-libraries) is vulnerable to DOM XSS via client-side prototype pollution. This is due to a gadget in a third-party library, which is easy to miss due to the minified source code. 

### Proof of concept

1. Load the lab in Burp's built-in browser.
2. Enable [DOM Invader](https://portswigger.net/burp/documentation/desktop/tools/dom-invader) and enable the prototype pollution option.
3. Open the browser DevTools panel, go to the **DOM Invader** tab, then reload the page.
4. Observe that **DOM Invader** has identified two prototype pollution vectors in the hash property i.e. the URL fragment string.
5. Click **Scan for gadgets**. A new tab opens in which **DOM Invader** begins scanning for gadgets using the selected source.
6. When the scan is complete, open the DevTools panel in the same tab as the scan, then go to the **DOM Invader** tab.
7. Observe that **DOM Invader** has successfully accessed the `setTimeout()` sink via the `hitCallback` gadget.
8. Click Exploit. **DOM Invader** automatically generates a proof-of-concept exploit and calls `alert(1)`.
9. Disable **DOM Invader**.
10. In the browser, go to the lab's exploit server.
11. In the Body section, craft an exploit that will navigate the victim to a malicious URL:

```text
<script>
    location="https://YOUR-LAB-ID.web-security-academy.net/#__proto__[hitCallback]=alert%28document.cookie%29"
</script>
```

12. Test the exploit on yourself, making sure that you're navigated to the lab's home page and that the `alert(document.cookie)` payload is triggered.
13. Go back to the exploit server and deliver the exploit to the victim to solve the lab.

### Exploitability

An attacker will need to use DOM Invader to identify a prototype pollution and a gadget for DOM XSS; and use the provided exploit server to deliver a payload to the victim that calls `alert(document.cookie)` in their browser.

Although it's technically possible to solve this lab manually, using [DOM Invader](https://portswigger.net/burp/documentation/desktop/tools/dom-invader) is recommended as this will save a considerable amount of time and effort. 

----

## Client-side prototype pollution via browser APIs

### Description

[This lab](https://portswigger.net/web-security/prototype-pollution/browser-apis/lab-prototype-pollution-client-side-prototype-pollution-via-browser-apis) is vulnerable to DOM XSS via client-side prototype pollution. The website's developers have noticed a potential gadget and attempted to patch it. However, you can bypass the measures they've taken. Also see [Widespread prototype pollution gadgets](https://portswigger.net/research/widespread-prototype-pollution-gadgets).

### Proof of concept

1. Load the lab in Burp's built-in browser.
2. Enable [DOM Invader](https://portswigger.net/burp/documentation/desktop/tools/dom-invader) and enable the prototype pollution option.
3. Open the browser DevTools panel, go to the **DOM Invader** tab, then reload the page.
4. Observe that **DOM Invader** has identified two prototype pollution vectors in the search property i.e. the query string.
5. Click **Scan for gadgets**. A new tab opens in which **DOM Invader** begins scanning for gadgets using the selected source.
6. When the scan is complete, open the DevTools panel in the same tab as the scan, then go to the **DOM Invader** tab.
7. Observe that **DOM Invader** has successfully accessed the `script.src` sink via the value gadget.
8. Click Exploit. **DOM Invader** automatically generates a proof-of-concept exploit and calls `alert(1)`.

### Exploitability

An attacker will need to find a source that you can use to add arbitrary properties to the global `Object.prototype`; identify a gadget property that allows you to execute arbitrary JavaScript; and combine these to call `alert()`.

----

## Client-side prototype pollution via flawed sanitization

### Description

[This lab](https://portswigger.net/web-security/prototype-pollution/preventing/lab-prototype-pollution-client-side-prototype-pollution-via-flawed-sanitization) is vulnerable to DOM XSS via client-side prototype pollution. Although the developers have implemented measures to prevent prototype pollution, these can be easily bypassed.

### Proof of concept

#### Find a prototype pollution source

1. In your browser, try polluting the `Object.prototype` by injecting an arbitrary property via the query string:

```text
/?__proto__.foo=bar
```

2. Open the browser DevTools panel and go to the Console tab.
3. Enter `Object.prototype`.
4. Study the properties of the returned object and observe that your injected foo property has not been added.
5. Try alternative prototype pollution vectors. For example:

```text
/?__proto__[foo]=bar
/?constructor.prototype.foo=bar
```

6. Observe in each instance the `Object.prototype` is not modified.
7. Go to the **Sources** tab and study the JavaScript files that are loaded by the target site. Notice that `deparamSanitized.js` uses the `sanitizeKey()` function defined in `searchLoggerFiltered.js` to strip potentially dangerous property keys based on a blocklist. However, it does not apply this filter recursively.
8. Back in the URL, try injecting one of the blocked keys in such a way that the dangerous key remains following the sanitization process. For example:

```text
/?__pro__proto__to__[foo]=bar
/?__pro__proto__to__.foo=bar
/?constconstructorructor.[protoprototypetype][foo]=bar
/?constconstructorructor.protoprototypetype.foo=bar
```

9. In the console, enter Object.prototype again. Notice that it now has its own foo property with the value bar. You've successfully found a prototype pollution source and bypassed the website's key sanitization.

#### Identify a gadget

1. Study the JavaScript files again and notice that `searchLogger.js` dynamically appends a script to the DOM using the config object's `transport_url` property if present.
2. Notice that no `transport_url` property is set for the `config` object. This is a potential gadget.

#### Craft an exploit

1. Using the prototype pollution source you identified earlier, try injecting an arbitrary `transport_url` property:

```text
/?__pro__proto__to__[transport_url]=foo
```

2. In the browser DevTools panel, go to the Elements tab and study the HTML content of the page. Observe that a `script` element has been rendered on the page, with the src attribute foo.
3. Modify the payload in the URL to inject an XSS proof-of-concept. For example, you can use a `data: URL`:

```text
/?__pro__proto__to__[transport_url]=data:,alert(1);
```

4. Observe that the `alert(1)` is called and the lab is solved.

### Exploitability

An attacker will need to find a source that you can use to add arbitrary properties to the global `Object.prototype`; identify a gadget property that allows you to execute arbitrary JavaScript; and combine these to call `alert()`.

