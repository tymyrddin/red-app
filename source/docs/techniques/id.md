# Insecure deserialisation

Serialization is the process by which some bit of data in a programming language gets converted into a format that allows it to be saved in a database or transferred over a network. Deserialisation refers to the opposite process, whereby the program reads the serialised object from a file or the network and converts it back into an object.

Insecure deserialisation is a type of vulnerability that arises when an attacker can manipulate the serialised object to cause unintended consequences in the program. This can lead to authentication bypasses or even [RCE](rce.md).

## Steps

1. If you can get access to an application’s source code, search for deserialisation functions in source code that accept user input.
2. If you cannot get access to source code, look for large blobs of data passed into an application. These could indicate serialised objects that are encoded.
3. Alternatively, look for features that might have to deserialise objects supplied by the user, such as database inputs, authentication tokens, and HTML form parameters.
4. If the serialised object contains information about the identity of the user, try tampering with the serialised object found and see if you can achieve authentication bypass.
5. See if you can escalate the flaw into an SQL injection or remote code execution. Be extra careful not to cause damage to the target application or server.
6. Draft report.

## Code review

Conducting a source code review is the most reliable way to detect deserialisation vulnerabilities.

## Other ways

It is also possible to find deserialisation vulnerabilities without examining any code.

Begin by paying close attention to the large blobs of data passed into an application. Large data blobs could be serialised objects that represent object injection opportunities. If the data is encoded, try to decode it. Most encoded data passed into web applications is encoded with `base64`.

Alternatively, start by seeking out features that are prone to deserialisation flaws. Look for features that might have to deserialise objects supplied by the user, such as database inputs, authentication tokens, and HTML form parameters.

Once you’ve found a user-supplied serialised object, you need to determine the type of serialised object it is. Is it a PHP object, a Python object, a Ruby object, or a Java object? Read each programming language’s documentation to familiarise yourself with the structure of its serialised objects.

## Escalation

Insecure deserialisation bugs often result in [remote code execution](rce.md), granting the attacker a wide range of capabilities with which to impact the application. And even when RCE is not possible, you might be able to achieve an authentication bypass or otherwise interfere with the logic flow of the application.

The impact of insecure deserialization can be limited when the vulnerability relies on an obscure point of entry, or requires a certain level of application privilege to exploit, or if the vulnerable function is not available to unauthenticated users.

When escalating deserialisation flaws, take the scope and rules of the pentesting assessment or bounty program into account. Deserialisation vulnerabilities can be dangerous, so make sure you don’t cause damage to the target application when trying to manipulate program logic or execute arbitrary code.

## Portswigger lab writeups

* [Modifying serialised objects](../deserialisation/1.md)
* [Modifying serialised data types](../deserialisation/2.md)
* [Using application functionality to exploit insecure deserialisation](../deserialisation/3.md)
* [Arbitrary object injection in PHP](../deserialisation/4.md)
* [Exploiting Java deserialisation with Apache Commons](../deserialisation/5.md)
* [Exploiting PHP deserialisation with a pre-built gadget chain](../deserialisation/6.md)
* [Exploiting Ruby deserialisation using a documented gadget chain](../deserialisation/7.md)
* [Developing a custom gadget chain for Java deserialisation](../deserialisation/8.md)
* [Developing a custom gadget chain for PHP deserialisation](../deserialisation/9.md)
* [Using PHAR deserialisation to deploy a custom gadget chain](../deserialisation/10.md)

## Remediation

* Some frameworks do not allow deserialising objects of arbitrary type. These frameworks will check the type of the input object and refuse to run any code if the type is unexpected. For example, Jackson will not allow you to deserialise objects of random types unless you explicitly turn that behaviour on.
* If the serialisation library used allows arbitrary types and this behaviour can not be turned off, consider validating the types before deserialising. For example, supplement Java’s built-in deserialisation API with an open source library like Apache Commons IO.
* Java "recently" (in 2021) got equipped with native solutions to solve deserialisation issues. Upgrade to at least 17.

## Resources

* [Portswigger: Insecure deserialization](https://portswigger.net/web-security/deserialization)
* [OWASP: A8:2017-Insecure Deserialization](https://owasp.org/www-project-top-ten/2017/A8_2017-Insecure_Deserialization)
* [New Java 17 features for improved security and serialization](https://snyk.io/blog/new-java-17-features-for-improved-security-and-serialization/)
* [Bug Bounty Bootcamp](https://nostarch.com/bug-bounty-bootcamp)
* [Bug Bounty Hunting Essentials](https://www.packtpub.com/product/bug-bounty-hunting-essentials/9781788626897)
* [Bug Bounty Hunting for Web Security](https://link.springer.com/book/10.1007/978-1-4842-5391-5)

