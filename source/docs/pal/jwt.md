# Flawed handling of JSON web tokens

## JWT authentication bypass via unverified signature

### Description

[This lab](https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-unverified-signature) uses a JWT-based mechanism for handling sessions. Due to implementation flaws, the server doesn't verify the signature of any JWTs that it receives. 

### Reproduction and proof of concept

1. In the lab, log in to your own account.
2. In Burp, go to the **Proxy -> HTTP history** tab and look at the post-login `GET /my-account` request. Observe that your session cookie is a JWT.
3. Double-click the payload part of the token to view its decoded JSON form in the Inspector panel. Notice that the `sub` claim contains your username. Send this request to Burp Repeater.
4. In Burp Repeater, change the path to `/admin` and send the request. Observe that the admin panel is only accessible when logged in as the administrator user.
5. Select the payload of the JWT again. In the Inspector panel, change the value of the `sub` claim from `wiener` to `administrator`, then click **Apply changes**.
6. Send the request again. Observe that you have successfully accessed the admin panel.
7. In the response, find the URL for deleting Carlos (`/admin/delete?username=carlos`). Send the request to this endpoint to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; modify the session token to gain access to the admin panel at `/admin`, then delete the user `carlos`. 

----

## JWT authentication bypass via flawed signature verification

### Description

[This lab](https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-flawed-signature-verification) uses a JWT-based mechanism for handling sessions. The server is insecurely configured to accept unsigned JWTs. 

### Reproduction and proof of concept

1. In the lab, log in to your own account.
2. In Burp, go to the **Proxy -> HTTP history** tab and look at the post-login `GET /my-account` request. Observe that your session cookie is a JWT.
3. Double-click the payload part of the token to view its decoded JSON form in the Inspector panel. Notice that the `sub` claim contains your username. Send this request to Burp Repeater.
4. In Burp Repeater, change the path to `/admin` and send the request. Observe that the admin panel is only accessible when logged in as the administrator user.
5. Select the payload of the JWT again. In the Inspector panel, change the value of the `sub` claim to administrator, then click **Apply changes**.
6. Select the header of the JWT, then use the Inspector to change the value of the `alg` parameter to `none`. Click **Apply changes**.
7. In the message editor, remove the signature from the JWT, but remember to leave the trailing dot after the payload.
8. Send the request and observe that you have successfully accessed the admin panel.
9. In the response, find the URL for deleting Carlos (`/admin/delete?username=carlos`). Send the request to this endpoint to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; modify the session token to gain access to the admin panel at `/admin`, then delete the user `carlos`. 

----

## JWT authentication bypass via weak signing key

### Description

[This lab](https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key) uses a JWT-based mechanism for handling sessions. It uses an extremely weak secret key to both sign and verify tokens. This can be easily brute-forced using a [wordlist of common secrets](https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list).

### Reproduction and proof of concept

#### Brute-force the secret key

1. In Burp, load the [JWT Editor extension](https://portswigger.net/bappstore/26aaa5ded2f74beea19e2ed8345a93dd) from the BApp store.
2. In the lab, log in to your own account and send the post-login `GET /my-account` request to Burp Repeater.
3. In Burp Repeater, change the path to `/admin` and send the request. Observe that the admin panel is only accessible when logged in as the administrator user.
4. Copy the JWT and brute-force the secret. You can do this using hashcat:

```text
hashcat -a 0 -m 16500 <YOUR-JWT> /path/to/jwt.secrets.list
```
    
5. If you're using hashcat, this outputs the JWT, followed by the secret. If everything worked correctly, this should reveal that the weak secret is `secret1`.

#### Generate a forged signing key

1. Using Burp Decoder, Base64 encode the secret that you brute-forced in the previous section.
2. In Burp, go to the **JWT Editor Keys** tab and click **New Symmetric Key**. In the dialog, click **Generate** to generate a new key in JWK format. Note that you don't need to select a key size as this will automatically be updated later.
3. Replace the generated value for the `k` property with the Base64-encoded secret.
4. Click OK to save the key.

#### Modify and sign the JWT

1. Go back to the `GET /admin` request in Burp Repeater and switch to the extension-generated **JSON Web Token message editor** tab.
2. In the payload, change the value of the `sub` claim to `administrator`.
3. At the bottom of the tab, click **Sign**, then select the key that you generated in the previous section.
4. Make sure that the `Don't modify header` option is selected, then click **OK**. The modified token is now signed with the correct signature.
5. Send the request and observe that you have successfully accessed the admin panel.
6. In the response, find the URL for deleting Carlos (`/admin/delete?username=carlos`). Send the request to this endpoint to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; brute-force the website's secret key; use it to sign a modified session token that gives access to the admin panel at `/admin`; then delete the user `carlos`. 

----

## JWT authentication bypass via jwk header injection

### Description

[This lab](https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jwk-header-injection) uses a JWT-based mechanism for handling sessions. The server supports the jwk parameter in the JWT header. This is sometimes used to embed the correct verification key directly in the token. However, it fails to check whether the provided key came from a trusted source. 

### Reproduction and proof of concept

1. In Burp, load the JWT Editor extension from the BApp store.
2. In the lab, log in to your own account and send the post-login `GET /my-account` request to Burp Repeater.
3. In Burp Repeater, change the path to `/admin` and send the request. Observe that the admin panel is only accessible when logged in as the administrator user.
4. Go to the **JWT Editor Keys** tab in Burp's main tab bar.
5. Click **New RSA Key**.
6. In the dialog, click Generate to automatically generate a new key pair, then click **OK** to save the key. Note that you don't need to select a key size as this will automatically be updated later.
7. Go back to the `GET /admin` request in Burp Repeater and switch to the extension-generated **JSON Web Token** tab.
8. In the payload, change the value of the `sub` claim to administrator.
9. At the bottom of the **JSON Web Token** tab, click **Attack**, then select **Embedded JWK**. When prompted, select your newly generated RSA key and click **OK**.
10. In the header of the JWT, observe that a `jwk` parameter has been added containing your public key.
11. Send the request. Observe that you have successfully accessed the admin panel.
12. In the response, find the URL for deleting Carlos (`/admin/delete?username=carlos`). Send the request to this endpoint to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; modify and sign a JWT that gives access to the admin panel at `/admin`; then delete the user `carlos`. 

Instead of using the built-in attack in the JWT Editor extension, you can embed a JWK by adding a `jwk` parameter to the header of the JWT manually. In this case, you need to also update the `kid` header of the token to match the `kid` of the embedded key. 

----

## JWT authentication bypass via jku header injection

### Description

[This lab](https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jku-header-injection) uses a JWT-based mechanism for handling sessions. The server supports the `jku` parameter in the JWT header. However, it fails to check whether the provided URL belongs to a trusted domain before fetching the key.

### Reproduction and proof of concept

#### Upload a malicious JWK Set

1. In Burp, load the [JWT Editor extension](https://portswigger.net/bappstore/26aaa5ded2f74beea19e2ed8345a93dd) from the BApp store.
2. In the lab, log in to your own account and send the post-login `GET /my-account` request to Burp Repeater.
3. In Burp Repeater, change the path to `/admin` and send the request. Observe that the admin panel is only accessible when logged in as the administrator user.
4. Go to the **JWT Editor Keys** tab in Burp's main tab bar.
5. Click **New RSA Key**.
6. In the dialog, click **Generate** to automatically generate a new key pair, then click **OK** to save the key. Note that you don't need to select a key size as this will automatically be updated later.
7. In the browser, go to the exploit server.
8. Replace the contents of the Body section with an empty **JWK Set**:

```text
{
    "keys": [

    ]
}
```

9. Back on the JWT Editor Keys tab, right-click on the entry for the key that you just generated, then select Copy Public Key as JWK.
10. Paste the JWK into the keys array on the exploit server, then store the exploit. The result should look something like this:

```text
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "893d8f0b-061f-42c2-a4aa-5056e12b8ae7",
            "n": "yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9mk6GPM9gNN4Y_qTVX67WhsN3JvaFYw"
        }
    ]
}
```

#### Modify and sign the JWT

1. Go back to the `GET /admin` request in Burp Repeater and switch to the extension-generated **JSON Web Token message** editor tab.
2. In the header of the JWT, replace the current value of the `kid` parameter with the `kid` of the JWK that you uploaded to the exploit server.
3. Add a new `jku` parameter to the header of the JWT. Set its value to the URL of your JWK Set on the exploit server.
4. In the payload, change the value of the `sub` claim to `administrator`.
5. At the bottom of the tab, click **Sign**, then select the RSA key that you generated in the previous section.
6. Make sure that the `Don't modify header` option is selected, then click **OK**. The modified token is now signed with the correct signature.
7. Send the request. Observe that you have successfully accessed the admin panel.
8. In the response, find the URL for deleting Carlos (`/admin/delete?username=carlos`). Send the request to this endpoint to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; forge a JWT that gives access to the admin panel at `/admin`; then delete the user carlos.

----

## JWT authentication bypass via kid header path traversal

### Description

[This lab](https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-kid-header-path-traversal) uses a JWT-based mechanism for handling sessions. In order to verify the signature, the server uses the `kid` parameter in JWT header to fetch the relevant key from its filesystem. 

### Reproduction and proof of concept

Generate a suitable signing key

1. In Burp, [JWT Editor extension](https://portswigger.net/bappstore/26aaa5ded2f74beea19e2ed8345a93dd) from the BApp store.
2. In the lab, log in to your own account and send the post-login `GET /my-account` request to Burp Repeater.
3. In Burp Repeater, change the path to `/admin` and send the request. Observe that the admin panel is only accessible when logged in as the administrator user.
4. Go to the **JWT Editor Keys** tab in Burp's main tab bar.
5. Click **New Symmetric Key**.
6. In the dialog, click Generate to generate a new key in JWK format. Note that you don't need to select a key size as this will automatically be updated later.
7. Replace the generated value for the `k` property with a Base64-encoded null byte (`AA==`).
8. Click **OK** to save the key.

#### Modify and sign the JWT

1. Go back to the `GET /admin` request in Burp Repeater and switch to the extension-generated **JSON Web Token message** editor tab.
2. In the header of the JWT, change the value of the `kid` parameter to a path traversal sequence pointing to the `/dev/null` file:

```text
../../../../../../../dev/null
```
    
3. In the JWT payload, change the value of the `sub` claim to `administrator`.
4. At the bottom of the tab, click **Sign**, then select the symmetric key that you generated in the previous section.
5. Make sure that the `Don't modify header` option is selected, then click **OK**. The modified token is now signed using a null byte as the secret key.
6. Send the request and observe that you have successfully accessed the admin panel.
7. In the response, find the URL for deleting Carlos (`/admin/delete?username=carlos`). Send the request to this endpoint to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; forge a JWT that gives access to the admin panel at `/admin`, then delete the user `carlos`. 

----

## JWT authentication bypass via algorithm confusion

### Description

[This lab](https://portswigger.net/web-security/jwt/algorithm-confusion/lab-jwt-authentication-bypass-via-algorithm-confusion) uses a JWT-based mechanism for handling sessions. It uses a robust RSA key pair to sign and verify tokens. However, due to implementation flaws, this mechanism is vulnerable to algorithm confusion attacks. 

### Reproduction and proof of concept

#### Obtain the server's public key

1. In Burp, load the [JWT Editor extension](https://portswigger.net/bappstore/26aaa5ded2f74beea19e2ed8345a93dd) from the BApp store.
2. In the lab, log in to your own account and send the post-login `GET /my-account` request to Burp Repeater.
3. In Burp Repeater, change the path to `/admin` and send the request. Observe that the admin panel is only accessible when logged in as the administrator user.
4. In the browser, go to the standard endpoint `/jwks.json` and observe that the server exposes a `JWK Set` containing a single public key.
5. Copy the JWK object from inside the keys array. Make sure that you don't accidentally copy any characters from the surrounding array.

#### Generate a malicious signing key

1. In Burp, go to the **JWT Editor Keys** tab in Burp's main tab bar.
2. Click **New RSA Key**.
3. In the dialog, make sure that the `JWK` option is selected, then paste the `JWK` that you just copied. Click **OK** to save the key.
4. Right-click on the entry for the key that you just created, then select **Copy Public Key as PEM**.
5. Use the **Decoder** tab to Base64 encode this PEM key, then copy the resulting string.
6. Go back to the **JWT Editor Keys** tab in Burp's main tab bar.
7. Click **New Symmetric Key**. In the dialog, click **Generate** to generate a new key in JWK format. Note that you don't need to select a key size as this will automatically be updated later.
8. Replace the generated value for the `k` property with a Base64-encoded PEM that you just created.
9. Save the key.

#### Modify and sign the token

1. Go back to the `GET /admin` request in Burp Repeater and switch to the extension-generated **JSON Web Token** tab.
2. In the header of the JWT, change the value of the `alg` parameter to `HS256`.
3. In the payload, change the value of the `sub` claim to `administrator`.
4. At the bottom of the tab, click **Sign**, then select the symmetric key that you generated in the previous section.
5. Make sure that the `Don't modify header` option is selected, then click **OK**. The modified token is now signed using the server's public key as the secret key.
6. Send the request and observe that you have successfully accessed the admin panel.
7. In the response, find the URL for deleting Carlos (`/admin/delete?username=carlos`). Send the request to this endpoint to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; obtain the server's public key. (exposed via a standard endpoint); use this key to sign a modified session token that gives access to the admin panel at `/admin`; then delete the user `carlos`. 

----

## JWT authentication bypass via algorithm confusion with no exposed key

### Description

[This lab](https://portswigger.net/web-security/jwt/algorithm-confusion/lab-jwt-authentication-bypass-via-algorithm-confusion-with-no-exposed-key) uses a JWT-based mechanism for handling sessions. It uses a robust RSA key pair to sign and verify tokens. However, due to implementation flaws, this mechanism is vulnerable to algorithm confusion attacks. 

### Reproduction and proof of concept

#### Obtain two JWTs generated by the server

1. In Burp, load the [JWT Editor extension](https://portswigger.net/bappstore/26aaa5ded2f74beea19e2ed8345a93dd) from the BApp store.
2. In the lab, log in to your own account and send the post-login `GET /my-account` request to Burp Repeater.
3. In Burp Repeater, change the path to `/admin` and send the request. Observe that the admin panel is only accessible when logged in as the `administrator` user.
4. Copy your JWT session cookie and save it somewhere for later.
5. Log out and log in again.
6. Copy the new JWT session cookie and save this as well. You now have two valid JWTs generated by the server.

#### Brute-force the server's public key

1. In a terminal, run the following command, passing in the two JWTs as arguments.

```text
docker run --rm -it portswigger/sig2n <token1> <token2>
```
    
Note that the first time you run this, it may take several minutes while the image is pulled from Docker Hub.

2. Notice that the output contains one or more calculated values of `n`. Each of these is mathematically possible, but only one of them matches the value used by the server. In each case, the output also provides the following:
* A Base64-encoded public key in both X.509 and PKCS1 format.
* A tampered JWT signed with each of these keys.
3. Copy the tampered JWT from the first X.509 entry (you may only have one).
4. Go back to your request in Burp Repeater and change the path back to `/my-account`.
5. Replace the session cookie with this new JWT and then send the request.
* If you receive a 200 response and successfully access your account page, then this is the correct X.509 key.
* If you receive a 302 response that redirects you to /login and strips your session cookie, then this was the wrong X.509 key. In this case, repeat this step using the tampered JWT for each X.509 key that was output by the script.

#### Generate a malicious signing key

1. From your terminal window, copy the Base64-encoded X.509 key that you identified as being correct in the previous section. Note that you need to select the key, not the tampered JWT that you used in the previous section.
2. In Burp, go to the **JWT Editor Keys** tab and click **New Symmetric Key**.
3. In the dialog, click **Generate** to generate a new key in JWK format.
4. Replace the generated value for the `k` property with a Base64-encoded key that you just copied. Note that this should be the actual key, not the tampered JWT that you used in the previous section.
5. Save the key.

#### Modify and sign the token

1. Go back to your request in Burp Repeater and change the path to `/admin`.
2. Switch to the extension-generated **JSON Web Token** tab.
3. In the header of the JWT, make sure that the `alg` parameter is set to `HS256`.
4. In the JWT payload, change the value of the `sub` claim to `administrator`.
5. At the bottom of the tab, click **Sign**, then select the symmetric key that you generated in the previous section.
6. Make sure that the `Don't modify header` option is selected, then click **OK**. The modified token is now signed using the server's public key as the secret key.
7. Send the request and observe that you have successfully accessed the admin panel.
8. In the response, find the URL for deleting Carlos (`/admin/delete?username=carlos`). Send the request to this endpoint to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; obtain the server's public key. Use this key to sign a modified session token that gives access to the admin panel at `/admin`, then delete the user `carlos`. 

