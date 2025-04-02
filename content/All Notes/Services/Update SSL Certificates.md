# Update SSL Certificates
28-03-2025
Tags: #services #html

- Installl certbot
- Run Certbot (make sure port 80 and 443 are open)
- Crontab default

```bash
0 */12 * * * root test -x /usr/bin/certbot -a \! -d /run/systemd/system && perl -e 'sleep int(rand(43200))' && certbot -q renew
```

- Auto renew cert script:

```bash
#!/bin/bash
set -e

# Fetch the number of days left until certificate expiration
days_left=$(echo $(( ( $(date -d "$(openssl s_client -connect kuma-ping.gamota.net:443 -servername kuma-ping.gamota.net < /dev/null 2>/dev/null | openssl x509 -noout -enddate | cut -d= -f2)" +%s) - $(date +%s) ) / 86400 )))

# If days left is less than 30, proceed with certbot renewal
if [ "$days_left" -lt 130 ]; then
  # Open firewall ports 80 and 443
  ufw allow 80/tcp
  ufw allow 443/tcp

  # Ensure the rules are removed after the script finishes
  trap "ufw delete allow 80/tcp; ufw delete allow 443/tcp" EXIT

  # Run certbot to renew the certificate
  certbot -q renew
else
  echo "Certificate has $days_left days remaining. No renewal needed."
fi
```

- Add in file /etc/cron.d/certbot

```bash
0 0 * * * root test -x /usr/share/renew-cert.sh && /usr/share/renew-cert.sh
```

- How to make a complete fullchain.pem
	- domain.pem
	- ChainCA1
	- ChainCA2
	- RootCA
- 1 Private Key
# References
- https://certbot.eff.org/
