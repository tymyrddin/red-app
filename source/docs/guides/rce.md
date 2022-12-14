# Remote code execution (RCE)

Remote code execution (RCE) occurs when an attacker can execute arbitrary code on a target machine because of a vulnerability or misconfiguration. RCEs are extremely dangerous, as attackers can often ultimately compromise the web application or even the underlying web server.

## Steps

1. Identify suspicious user-input locations. For code injections, take note of every user-input location, including URL parameters, HTTP headers, body parameters, and file uploads. To find potential file inclusion vulnerabilities, check for input locations being used to determine or construct filenames and for file-upload functions.
2. Submit test payloads to the input locations in order to detect potential vulnerabilities.
3. If your requests are blocked, try protection-bypass techniques and see if your payload succeeds.
4. Finally, confirm the vulnerability by trying to execute harmless commands such as `whoami`, `ls`, and `sleep 5`.
5. Avoid reading sensitive system files or altering any files with the vulnerability found.
6. Draft report.

## Gather information about the target

The first step to finding any vulnerability is to [gather information](../recon/README.md) about the target. When hunting for RCEs, this step is especially important because the route to achieving an RCE is extremely dependent on the way the target is built. You should find out information about the web server, programming language, and other technologies used by your current target.

## Identify Suspicious User Input Locations

As with finding many other vulnerabilities, the next step to finding any RCE is to identify the locations where users can submit input to the application. When hunting for code injections, take note of every direct user-input location, including URL parameters, HTTP headers, body parameters, and file uploads. Sometimes applications parse user-supplied files and concatenate their contents unsafely into executed code, so any input that is eventually passed into commands is something you should look out for. 

To find potential file inclusion vulnerabilities, check for input locations being used to determine filenames or paths, as well as any file-upload functionalities in the application.

## Submit test payloads

The next thing you should do is to submit test payloads to the application. For code injection vulnerabilities, try payloads that are meant to be interpreted by the server as code and see if they get executed.

## Confirm the vulnerability

Finally, confirm the vulnerability by executing harmless commands like `whoami`, `ls`, and `sleep 5`.

## Escalation

Be extra cautious when escalating RCE vulnerabilities. Most companies would prefer that you do not try to escalate them at all because they do not want someone poking around systems that contain confidential data. 

During a typical penetration test, a hacker will often try to figure out the privileges of the current user and attempt privilege-escalation attacks after they gain RCE.

And in a bug bounty context, this is not appropriate. You might accidentally read sensitive information about customers or cause damage to the systems by modifying a critical file. It is important to carefully read the bounty program rules, so you do not cross the lines.

For classic RCEs, create a proof of concept that executes a harmless command like `whoami` or `ls`. You can also prove you???ve found an RCE by reading a common system file such as `/etc/passwd`.

## Bypassing protections

Many applications have caught on to the dangers of RCE and employ either input validation or a firewall to stop potentially malicious requests. But programming languages are often quite flexible, and that enables us to work within the bounds of the input validation rules to make our attack work.

For Unix system commands, you can insert quotes and double quotes without changing the command???s behaviour. You can also use wildcards to substitute for arbitrary characters if the system is filtering out certain strings. And any empty command substitution results can be inserted into the string without changing the results. For example, these will all print the contents of `/etc/shadow`:

    cat /etc/shadow
    cat "/e"tc'/shadow'
    cat /etc/sh*dow
    cat /etc/sha``dow
    cat /etc/sha$()dow
    cat /etc/sha${}dow

You can hex-encode, URL-encode, double-URL-encode, and vary the cases (uppercase or lowercase characters) of payloads. You can try to insert special characters such as null bytes, newline characters, escape characters (`\`), and other special or non-ASCII characters into the payload. Then, observe which payloads are blocked and which ones succeed, and craft exploits that will bypass the filter to accomplish the desired results.

## Resources

* [Portswigger Lab: Remote code execution via web shell upload](https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-web-shell-upload)
* [Bug Bounty Bootcamp](https://nostarch.com/bug-bounty-bootcamp)
* [Bug Bounty Hunting Essentials](https://www.packtpub.com/product/bug-bounty-hunting-essentials/9781788626897)
* [Bug Bounty Hunting for Web Security](https://link.springer.com/book/10.1007/978-1-4842-5391-5)


