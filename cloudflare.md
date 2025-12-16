# __Cloudflare__

---

## ***mod_remoteip***

<https://developers.cloudflare.com/support/troubleshooting/restoring-visitor-ips/restoring-original-visitor-ips/>

_This will make apache log to show the real IP addresses and not the cloudflare ones_

---

### ***Enable mod_remoteip***

```bash
sudo a2enmod remoteip
```

### ___Configure vhosts___

Update the site configuration to include RemoteIPHeader CF-Connecting-IP, e.g. `/etc/apache2/sites-available/000-default.conf`

```apache
ServerAdmin webmaster@localhost
DocumentRoot /var/www/html
ServerName remoteip.andy.support
RemoteIPHeader CF-Connecting-IP
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
```
### ___Configure apache2.conf___

Update combined LogFormat entry in apache.conf, replacing `%h` with `%a` in `/etc/apache2/apache2.conf`

```bash
sed -i 's|%h|%a|g' /etc/apache2/apache2.conf
```

### ___Create remoteip.conf___

---

Define trusted proxy addresses by creating `/etc/apache2/conf-available/remoteip.conf`

#### ___Manual___

Example:

```apache
RemoteIPHeader CF-Connecting-IP
RemoteIPTrustedProxy 192.0.2.1 (example IP address)
RemoteIPTrustedProxy 192.0.2.2 (example IP address)
(repeat for all Cloudflare IPs listed at https://www.cloudflare.com/ips/)
```

Ip list:

<https://www.cloudflare.com/ips/>

#### ___With Script___

Auto generage script:

generate_remoteip.conf.sh

<https://raw.githubusercontent.com/makemegit/generate_remoteip.conf.sh/refs/heads/main/generate_remoteip.conf.sh>

### ___Enable Apache configuration___

```bash
sudo a2enconf remoteip
```

```bash
systemctl restart apache2
```
