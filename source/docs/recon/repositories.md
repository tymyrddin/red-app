# Eyeing repositories

## Attack tree

```text
1 Look for hardcoded secrets
    1.1 SQL passwords
    1.2 AWS access keys
    1.3 Google Cloud private keys
    1.4 API tokens
    1.5 Test accounts
    1.6 ...
```

## Github

In GitHub search:

```text
# Sample of GitHub queries
org:TargetName password
org:TargetName aws_secret_access_key
org:TargetName aws_key
org:TargetName BEGIN RSA PRIVATE KEY
org:TargetName BEGIN OPENSSH PRIVATE KEY
org:TargetName secret_key
org:TargetName hooks.slack.com/services
org:TargetName sshpass -p
org:TargetName sq0csp
org:TargetName apps.googleusercontent.com
org:TargetName extension:pem key
```