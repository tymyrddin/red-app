# Insecure deserialization

Serialization is the process by which some bit of data in a programming language gets converted into a format that allows it to be saved in a database or transferred over a network. Deserialization refers to the opposite process, whereby the program reads the serialized object from a file or the network and converts it back into an object.

Insecure deserialization is a type of vulnerability that arises when an attacker can manipulate the serialized object to cause unintended consequences in the program. This can lead to authentication bypasses or even [RCE](rce.md).

## Steps

1. If you can get access to an application’s source code, search for deserialization functions in source code that accept user input.
2. If you cannot get access to source code, look for large blobs of data passed into an application. These could indicate serialized objects that are encoded.
3. Alternatively, look for features that might have to deserialize objects supplied by the user, such as database inputs, authentication tokens, and HTML form parameters.
4. If the serialized object contains information about the identity of the user, try tampering with the serialized object found and see if you can achieve authentication bypass.
5. See if you can escalate the flaw into a SQL injection or remote code execution. Be extra careful not to cause damage to your target application or server.
6. Draft report.

## Code review

Conducting a source code review is the most reliable way to detect deserialization vulnerabilities.

## Other ways

And it is possible to find deserialization vulnerabilities without examining any code.

Begin by paying close attention to the large blobs of data passed into an application. Large data blobs could be serialized objects that represent object injection opportunities. If the data is encoded, try to decode it. Most encoded data passed into web applications is encoded with base64.

Alternatively, you can start by seeking out features that are prone to deserialization flaws. Look for features that might have to deserialize objects supplied by the user, such as database inputs, authentication tokens, and HTML form parameters.

Once you’ve found a user-supplied serialized object, you need to determine the type of serialized object it is. Is it a PHP object, a Python object, a Ruby object, or a Java object? Read each programming language’s documentation to familiarise yourself with the structure of its serialized objects.

## Escalation

Insecure deserialization bugs often result in [remote code execution](rce.md), granting the attacker a wide range of capabilities with which to impact the application. And even when RCE is not possible, you might be able to achieve an authentication bypass or otherwise interfere with the logic flow of the application.

The impact of insecure deserialization can be limited when the vulnerability relies on an obscure point of entry, or requires a certain level of application privilege to exploit, or if the vulnerable function is not available to unauthenticated users.

When escalating deserialization flaws, take the scope and rules of the pentesting assessment or bounty program into account. Deserialization vulnerabilities can be dangerous, so make sure you don’t cause damage to the target application when trying to manipulate program logic or execute arbitrary code.

## Resources

* [Portswigger: Insecure deserialization](https://portswigger.net/web-security/deserialization)
* [OWASP: A8:2017-Insecure Deserialization](https://owasp.org/www-project-top-ten/2017/A8_2017-Insecure_Deserialization)
* [Bug Bounty Bootcamp](https://nostarch.com/bug-bounty-bootcamp)
* [Bug Bounty Hunting Essentials](https://www.packtpub.com/product/bug-bounty-hunting-essentials/9781788626897)
* [Bug Bounty Hunting for Web Security](https://link.springer.com/book/10.1007/978-1-4842-5391-5)

