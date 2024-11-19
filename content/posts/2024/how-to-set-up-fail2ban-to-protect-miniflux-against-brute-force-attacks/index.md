+++
title = 'How to Set Up Fail2ban to Protect Miniflux Against Brute Force Attacks'
description = 'This guide will walk you through the steps of setting up Fail2ban to automatically ban IP addresses that attempt to log in to your Miniflux instance too many times'
date = 2024-11-11T23:16:34-03:00
tags = ['Self-Hosting', 'Security']
draft = false
+++

If you self-host your own instance of Miniflux (a RSS Feed Reader) and have it exposed publicly to the internet, it is a good idea to protect it against brute force attacks.

I'm not going into details on how to install it, as it's already well documented on {{< url "the official webpage" "https://miniflux.app/docs/installation.html" >}}.

If you can afford it, an even better alternative before keep reading to this blogpost until the end, is to have it managed for you by the Miniflux team itself. Paying for a subscription of $15 bucks/year (as of writing) helps supporting product development, and skips all the hassle of managing updates, patches, ssl certificates and so on.

There are several ways to {{< url "install fail2ban" "https://github.com/fail2ban/fail2ban" >}}. Since my vps runs Ubuntu, I used the following command:

```bash
sudo apt install fail2ban
```

On my vps, Miniflux outputs its logs to `/var/log/syslog`, which can be accessed with:

```bash
sudo journalctl -u miniflux
```

I wanted Fail2ban to monitor these logs for failed authentication, and ban any IP address with three consecutive failed attempts for a few hours, so I did the following:

### 1. Created a custom Fail2ban filter to match log lines in `/var/log/syslog` that contain "miniflux" and "authentication_failed=true".

a. Create a custom filter for Miniflux:

```bash
sudo nano /etc/fail2ban/filter.d/miniflux.conf
```

b. Add the following filter definition:

 ```ini
 [Definition]

# Regex to match the relevant log lines
# <HOST> is a Fail2Ban placeholder that will capture the offending IP address from the log line.
failregex = .*miniflux.*authentication_failed=true.*client_ip=<HOST>

# This pattern should not match successful logins or other irrelevant logs
ignoreregex =
```

### 2. Configured Fail2ban Jail to look for three consecutive failed attempts from the same IP address and then ban that IP across all ports for a few hours.

a. Create a new jail for Miniflux:

 ```bash
 sudo nano /etc/fail2ban/jail.local
```

b. Add the following jail configuration:

 ```ini
 [miniflux]
enabled = true
port = 0:65535
filter = miniflux
logpath = /var/log/syslog
maxretry = 3
# 72 hours in seconds
bantime = 259200
# Window of time to consider failures (10 minutes in seconds)
findtime = 600
```

Lines explained:

- `enabled = true` activates the jail.
- `port = 0:65535` bans the IP for all ports.
- `filter = miniflux` specifies the custom filter we created.
- `logpath = /var/log/syslog` tells Fail2Ban to scan the syslog file.
- `maxretry = 3` specifies that the IP will be banned after three consecutive failed attempts.
- `bantime = 259200` sets the ban duration
- `findtime = 600` defines the window for detecting failures.

### 3. With everything in place, I tested and restarted Fail2ban.

a. Check your filter syntax for errors:

 ```bash
 sudo fail2ban-regex /var/log/syslog /etc/fail2ban/filter.d/miniflux.conf
```

b. Restart the Fail2Ban service to apply the new configuration:

```bash
sudo systemctl restart fail2ban
```
c. Monitor Fail2Ban logs to verify that the jail is working:

```bash
sudo tail -f /var/log/fail2ban.log
```
you can also check with the command:

```bash
$ sudo fail2ban-client status miniflux

my-user@my-vps:~$ sudo fail2ban-client status miniflux
Status for the jail: miniflux
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     0
|  `- Journal matches:
`- Actions
   |- Currently banned: 1
   |- Total banned:     1
   `- Banned IP list:   89.115.48.22
```

**Testing**: Be cautious during testing to avoid getting locked out of your vps. Use a VPN connection or connect via mobile network instead of your home network.