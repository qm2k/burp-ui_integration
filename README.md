# Integration files for [BURP-UI](https://burp-ui.readthedocs.io/en/latest/index.html)

## Usage
Please _don't_ blindly copy these files to your `/etc` directory, read them and make sure they do what you want.
Read [prerequisites](#prerequisites) section before proceeding and correct files accordingly if your system is different.

## Prerequisites
- BURP-UI is [configured to work with gunicorn](https://burp-ui.readthedocs.io/en/latest/gunicorn.html)
- BURP-UI/gunicorn service user is called `burp-ui`
- gunicorn PID file is `/var/run/burp-ui/gunicorn.pid` (PID file is required for correct log rotation)
- log files are `/var/log/gunicorn/burp-ui_{access,error,info}.log`

## File descriptions

### etc/logrotate.d/gunicorn_burp-ui
Rotates access logs daily, other logs monthly or after 1MB.

### etc/tmpfiles.d/burp-ui.conf
Creates empty directory `/var/run/burp-ui/` belonging to user `burp-ui` at boot
(creating PID file directly does not work due to gunicorn architecture).

### etc/fail2ban/filter.d/burp-ui-auth.conf
Filter that catches "Wrong username or password" messages in info log.

### etc/fail2ban/jail.burp-ui
Enables `burp-ui-auth` filter for `/var/log/gunicorn/burp-ui_info.log`, blocks ports 80 and 8080 when triggered.
Include contents of this file to your `/etc/fail2ban/jail.local`.
