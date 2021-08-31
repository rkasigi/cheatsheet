# SSH Cheat Sheet

## SSH login via jumphost (sock proxy)
```
ssh -o ProxyCommand="ssh -W %h:%p username@jumphost -p 22" username@target-host -p 22
```

## Find SSH Failed login

```
grep "Failed password" /var/log/secure* | grep -v "Failed password for invalid user" | awk '{print $11}' | sort | uniq -c | sort -nr

((grep "Failed password" /var/log/secure* | grep -v "Failed password for invalid user" | awk '{print $11}'); (grep "Failed password for invalid user" /var/log/secure* | awk '{print $13}')) | sort | uniq -c | sort -nr > loginAttempt.txt
```
