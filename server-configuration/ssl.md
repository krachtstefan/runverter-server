# SSL configuration

The ssl certificate was signed by [Let’s Encrypt](https://letsencrypt.org/) via [Certbot](https://certbot.eff.org/) on a Ubuntu 16.04 machine.

## Obtain a Certificate

Install letsencrypt

```shell
$ sudo apt-get install letsencrypt
```

Make sure haproxy is running and optain a certificate

```shell
$ letsencrypt certonly --webroot -w /root/runverter -d runverter.io
```

Combine fullchain.pem and privkey.pem to make it compatible with haproxy

```shell
$ DOMAIN='runverter.io' sudo -E bash -c 'cat /etc/letsencrypt/live/$DOMAIN/fullchain.pem /etc/letsencrypt/live/$DOMAIN/privkey.pem > /etc/haproxy/certs/$DOMAIN.pem'
```

Secure access to the combined file, which contains the private key

```shell
sudo chmod -R go-rwx /etc/haproxy/certs
```

## Set Up Auto Renewal

Let’s Encrypt certificates are valid for 90 days, but `letsencrypt renew` renews certificates that expire in less than 30 days to allow a margin of error.

use `crontab -e` to setup a proper renew cron

```
# run renew every monday 2:30 (3:30 in CEST)
30 2 * * 1  /usr/bin/docker stop $(/usr/bin/docker ps -aqf "name=runverter-haproxy") >> /var/log/letsencrypt-renew.log && /usr/bin/docker stop $(/usr/bin/docker ps -aqf "name=runverter-app") >> /var/log/letsencrypt-renew.log && /usr/bin/letsencrypt renew >> /var/log/letsencrypt-renew.log && /bin/cat /etc/letsencrypt/live/runverter.io/fullchain.pem /etc/letsencrypt/live/runverter.io/privkey.pem > /etc/haproxy/certs/runverter.io.pem && /usr/bin/docker start $(/usr/bin/docker ps -aqf "name=runverter-app") >> /var/log/letsencrypt-renew.log && /usr/bin/docker start $(/usr/bin/docker ps -aqf "name=runverter-haproxy") >> /var/log/letsencrypt-renew.log
```
