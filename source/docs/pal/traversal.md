# Directory traversal

## File path traversal, simple case

### Description

[This lab](https://portswigger.net/web-security/file-path-traversal/lab-simple) contains a file path traversal vulnerability in the display of product images.

### Proof of Concept

1. Use Burp Suite to intercept and modify a request that fetches a product image.
2. Modify the ``filename`` parameter, giving it the value:

```
../../../etc/passwd
```

3. Observe that the response contains the contents of the ``/etc/passwd`` file.

### Exploitability

An attacker will need to retrieve the contents of the `/etc/passwd` file.

----

## File path traversal, traversal sequences blocked with absolute path bypass

### Description

[This lab](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass) contains a file path traversal vulnerability in the display of product images. The application blocks traversal sequences but treats the supplied filename as being relative to a default working directory. 

### Proof of Concept

1. Use Burp Suite to intercept and modify a request that fetches a product image.
2. Modify the ``filename`` parameter, giving it the value ``/etc/passwd``.
3. Observe that the response contains the contents of the ``/etc/passwd`` file.

### Exploitability

An attacker will need to retrieve the contents of the `/etc/passwd` file. 

----

## File path traversal, traversal sequences stripped non-recursively

### Description

[This lab](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively) contains a file path traversal vulnerability in the display of product images. The application strips path traversal sequences from the user-supplied filename before using it.

### Proof of Concept

1. Use Burp Suite to intercept and modify a request that fetches a product image.
2. Modify the ``filename`` parameter, giving it the value:

```
....//....//....//etc/passwd
```

3. Observe that the response contains the contents of the ``/etc/passwd`` file.

### Exploitability

An attacker will need to retrieve the contents of the `/etc/passwd` file.

----

## File path traversal, traversal sequences stripped with superfluous URL-decode

### Description

[This lab](https://portswigger.net/web-security/file-path-traversal/lab-superfluous-url-decode) contains a file path traversal vulnerability in the display of product images. The application blocks input containing path traversal sequences. It then performs a URL-decode of the input before using it. 

### Proof of Concept

1. Use Burp Suite to intercept and modify a request that fetches a product image.
2. Modify the ``filename`` parameter, giving it the value:

```
..%252f..%252f..%252fetc/passwd
```

3. Observe that the response contains the contents of the ``/etc/passwd`` file.

### Exploitability

An attacker will need to encode the payload to retrieve the contents of the `/etc/passwd` file

----

## File path traversal, validation of start of path

### Description

[This lab](https://portswigger.net/web-security/file-path-traversal/lab-validate-start-of-path) contains a file path traversal vulnerability in the display of product images. The application transmits the full file path via a request parameter, and validates that the supplied path starts with the expected folder. 

### Proof of Concept

1. Use Burp Suite to intercept and modify a request that fetches a product image.
2. Modify the ``filename`` parameter, giving it the value:

```
/var/www/images/../../../etc/passwd
```

3. Observe that the response contains the contents of the ``/etc/passwd`` file.

### Exploitability

An attacker will need to retrieve the contents of the `/etc/passwd` file. 

----

## File path traversal, validation of file extension with null byte bypass

### Description

[This lab](https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass) contains a file path traversal vulnerability in the display of product images. The application validates that the supplied filename ends with the expected file extension. 

### Proof of Concept

1. Use Burp Suite to intercept and modify a request that fetches a product image.
2. Modify the ``filename`` parameter, giving it the value:

```
../../../etc/passwd%00.png
```

3. Observe that the response contains the contents of the ``/etc/passwd`` file.

### Exploitability

An attacker will need to retrieve the contents of the `/etc/passwd` file. 
