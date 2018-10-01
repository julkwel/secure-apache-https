# secure-apache-https
How to secure apache server with letsencrypt

Debian Jessie && + 2014
__
The easy ways is using **certbot-auto** this is work everywhere

wget https://dl.eff.org/certbot-auto

chmod a+x certbot-auto

sudo ./certbot-auto --apache

To renew your certificates use 

*sudo ./certbot-auto renew --dry-run *

Or using crontab

0 0,12 * * * python -c 'import random; import time; time.sleep(random.random() * 3600)' && ./certbot-auto renew


**Using certbot**

Edit your source-list 

 nano /etc/apt/sources.list
 
 Add this 
 
 *deb http://ftp.debian.org/debian stretch-backports main*
 
 Then update your package manager apt update
 
 Install the generator
 
 *sudo apt install python-certbot-apache -t stretch-backports *
 
 Setting up your configuration 
 
 *nano /etc/apache2/sites-available/your.site.conf*
 
 and add the ServerName *Your_server_name*
 
 Then reload the webServer
 
 *service apache2 reload*
 
 Allow HTTPS trafic in the firewall 
 
ufw status && ufw allow 'WWW Full' && ufw delete allow 'WWW' && ufw status

Then secure your domaine name 

 certbot --apache -d yourSite.com -d www.yourSite.com
 
 To renew run 
 
 *certbot renew --dry-run*  #Or add this in your crontab



If you need to redirect all request in https  add this in your configuration

```
RewriteEngine On
RewriteCond %{SERVER_PORT} !443
RewriteRule ^(/(.*))?$ https://%{HTTP_HOST}/$1 [R=301,L]
```




