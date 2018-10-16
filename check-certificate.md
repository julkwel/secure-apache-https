______________________________
**Vérification certificat SSL **

openssl pkey -in your_private.key -pubout -outform pem | sha256sum 

openssl x509 -in your_certificate.crt -pubkey -noout -outform pem | sha256sum 

openssl req -in your_publicKey.csr -pubkey -noout -outform pem | sha256sum

______________________________

** Create CA-BUNDLE **

__

Linux

cat ComodoRSAAddTrustCA.crt ComodoRSADomain/Organization/ExtendedvalidationSecureServerCA.crt AddTrustExternalCARoot.crt > yourDomain.ca-bundle

OR

cat ComodoSHA256SecureServerCA.crt AddTrustExternalCARoot.crt > yourDomain.ca-bundle

__

Windows 

copy ComodoRSAAddTrustCA.crt + ComodoRSADomain/Organization/ExtendedvalidationSecureServerCA.crt + AddTrustExternalCARoot.crt yourDomain.ca-bundle

OR

copy ComodoSHA256SecureServerCA.crt + AddTrustExternalCARoot.crt yourDomain.ca-bundle

______________________________

Apache install 

```
<VirtualHost *:443> 
  ServerName example.com 
  DocumentRoot /var/www/

  SSLEngine on 
  SSLCertificateFile /etc/pki/tls/certs/example_com.crt 
  SSLCertificateKeyFile /etc/pki/tls/certs/example_com.key 
  SSLCertificateChainFile /etc/pki/tls/certs/example_com.ca-bundle 
</VirtualHost>
```
