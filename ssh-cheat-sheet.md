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

## SSH Sock Proxy
```
ssh -D 1337 -q -C -N user@server -p 22
```
What that command does is;

1. -D 1337: open a SOCKS proxy on local port :1337. If that port is taken, try a different port number. If you want to open multiple SOCKS proxies to multiple endpoints, choose a different port for each one.
2. -C: compress data in the tunnel, save bandwidth
3. -q: quiet mode, don’t output anything locally
4. -N: do not execute remote commands, useful for just forwarding ports
5. user@server: the remote SSH server you have access to
6. -p port number

Once you run that, ssh will stay in the foreground until you CTRL+C it to cancel it. If you prefer to keep it running in the background, add -f to fork it to a background command.
