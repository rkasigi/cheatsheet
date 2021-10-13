# Install fail2ban on Centos 7

## Install fail2ban
```
sudo yum install -y epel-release
sudo yum install -y fail2ban
sudo systemctl enable fail2ban
```

## Configure sshd
Create a file `/etc/fail2ban/jail.d/sshd.conf` with the content below.
Please modify `port` depending on your needs.
```
[sshd]
enabled = true
port = 2522
mode   = aggressive
#action = firewallcmd-ipset
logpath = %(sshd_log)s
maxretry = 6
bantime = -1
findtime  = 48h
```

## Restart the fail2ban service
```
sudo systemctl restart fail2ban
```

## Get fail2ban status
The systemctl command should finish without any output. In order to check that the service is running, we can use fail2ban-client:
```
sudo fail2ban-client status
```
You can also get more detailed information about a specific jail:
```
sudo fail2ban-client status sshd
```

## Monitor Fail2ban Logs and Firewall Configuration
It’s important to know that a service like Fail2ban is working as-intended. Start by using systemctl to check the status of the service:
```
sudo systemctl status fail2ban
```

If something seems amiss here, you can troubleshoot by checking logs for the fail2ban unit since the last boot:
```
sudo journalctl -b -u fail2ban
```
 
Follow Fail2ban’s log for a record of recent actions (press Ctrl-C to exit):
```
sudo tail -F --tail 100 /var/log/fail2ban.log
```

List the current rules configured for iptables:
```
sudo iptables -nL
```
