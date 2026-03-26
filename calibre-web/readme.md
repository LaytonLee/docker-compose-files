# Calibre web podman config

### fail2ban

Install fail2ban:

```bash
sudo apt install fail2ban
```

Add calibre login request filter: `vim /etc/fail2ban/filter.d/nginx-calibre.conf`

```ini
[Definition]
failregex = ^<HOST> -.*"POST /login.* HTTP/.*" (401|302|200)
            ^<HOST> -.*"GET /login.* HTTP/.*" 401
```

Set calibre login traffic ban rules: `vim /etc/fail2ban/jail.local`:

```ini
[nginx-calibre]
enabled  = true
port     = http,https
filter   = nginx-calibre
logpath  = /var/log/nginx/calibre_access.log
maxretry = 5
findtime = 10m
bantime  = 1h
backend  = auto
```

Send some error login request, test the regex:

```bash
fail2ban-regex /var/log/nginx/calibre_access.log /etc/fail2ban/filter.d/nginx-calibre.conf
```

If result shows "Matches", Reload fail2ban to enable the filter:

```bash
sudo fail2ban-client reload
```

Check calibre filter status:

```bash
sudo fail2ban-client status nginx-calibre
```
