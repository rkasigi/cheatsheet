# SSH Cheat Sheet

## SSH login via jumphost (sock proxy)
```
ssh -o ProxyCommand="ssh -W %h:%p username@jumphost -p 22" username@target-host -p 22
```
