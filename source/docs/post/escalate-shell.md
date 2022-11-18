# Escalate a reverse shell to a more powerful shell

## Attack tree

```text
1 Enumerate environment and current privileges (AND)
2 Generate backdoor (with weevely for example) (.txt) (AND)
3 Upload to any server (with a direct URL) (AND)
4 Download it to target server (AND)
5 Rename the backdoor to original extension (AND)
6 Start listener on hacking machine (AND)
7 Connect to hacking machine from target server (AND)
8 Post exploitation
```

## Resources

* [pentestmonkey: Reverse Shell Cheat Sheet](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
* [Using Whois and Finger for Reverse Shells](https://lekstu.ga/posts/whois-and-finger-reverse-shell/)
* [Reverse Shell Cheat Sheet: PHP, Python, Powershell, Bash, NC, JSP, Java, Perl](https://highon.coffee/blog/reverse-shell-cheat-sheet/)

### Binaries

* [GTFOBins (Unix binaries)](https://gtfobins.github.io/)
* [LOLBAS (Windows binaries)](https://lolbas-project.github.io/)

### Auto-generated shells

* [Reverse shell generator](https://www.revshells.com/)
* [Shellerator](https://github.com/ShutdownRepo/shellerator)

### Obfuscation

* [Pyminifier](https://liftoff.github.io/pyminifier/pyminifier.html)

