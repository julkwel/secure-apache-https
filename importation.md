```
1. sudo a2enmod ssl
__

2. service apache2 restart
__

3. mkdir /etc/ssl/votre-domaine-fr/
__

4. netstat -tanpu | grep "LISTEN" | grep "443"
__

5. grep -R "Listen" /etc/apache2
__

6. Ajouter cette configuration sur apache si il n'écoute le port 443
<IfModule mod_ssl.c>
  Listen 443
</IfModule>
__

6. service apache2 restart
__

7. netstat -tanpu | grep "LISTEN" | grep "443"
__

8. grep -R "votre-adresse-deja-accessible-en-http.fr" /etc/apache2
__

9. cp your-vhost.conf your-vhost-ssl.conf
__

10. Editer le your-vhost-ssl.conf et changer le port *80 en *443
<VirtualHost *:443> 
    ServerName domain.com
    ServerAlias your.domain.com
    ServerAdmin webmaster@serveradmin.com
    DocumentRoot /path/to/your/project
      <Directory /path/to/your/project>
        Options -Indexes +FollowSymLinks +MultiViews
        AllowOverride none
        Require all granted
      </Directory>
      
# On active le SSL
SSLEngine On

# On active tous les protocoles (TLS v1.0, TLS v1.1 et TLS v1.2), mais on désactive SSL v2 et v3 (obsolètes et remplacés par TLS)
SSLProtocol All -SSLv3 -SSLv2

# On active les méthodes de chiffrement, et on désactive les méthodes de chiffrement non sécurisés (par la présente d'un !)
SSLCipherSuite HIGH:!aNULL:!MD5:!ADH:!RC4:!DH

# On demande au navigateur de sélectionner une méthode de chiffrement en respectant l'ordre envoyée par le serveur (HIGH uniquement)
SSLHonorCipherOrder on

# On renseigne le chemin vers le certificat SSL de l'adresse à sécuriser
SSLCertificateFile "/etc/ssl/votre-domaine-fr/www-votre-domaine-fr.cer"

# On renseigne le chemin vers la clée privée correspondant au certificat SSL de l'adresse à sécuriser
SSLCertificateKeyFile "/etc/ssl/votre-domaine-fr/www-votre-domaine-fr.key"

# On renseigne le chemin vers le certificat SSL racine, puis vers le(s) certificat(s) SSL intermédiaire(s).
# Si vous disposez de plusieurs certificats intermédiaires, vous pouvez ajouter d'autres directives SSLCACertificateFile.
SSLCACertificateFile "/etc/ssl/votre-domaine-fr/certificat-racine.cer"
SSLCACertificateFile "/etc/ssl/votre-domaine-fr/certificat-intermediaire.cer"

</VirtualHost>
__

11. a2ensite your-vhost-ssl.conf

12. service apache2 reload

__

** Comodo **
SSLEngine on
SSLCertificateFile /path/to/your_domain_name.crt
SSLCertificateKeyFile /path/to/your_private.key
SSLCertificateChainFile /path/to/ComodoCA.crt

.SSLCertificateFile votre dommaine certification (eg. votre_dommaine.crt)
.SSLCertificateKeyFile votr clé privé
.SSLCertificateChainFile Clé intérmediaire (ComodoRSACA.crt)


```
