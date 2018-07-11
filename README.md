# Request SSL certificate and configure AWS loadbalancer

## Request a ssl certificate (i.e. Comodo, Symantec)

1. openssl req -utf8 -nodes -sha256 -newkey rsa:2048 -keyout domainname_com.key -out domainname_com.csr
2. cat domainname_com.csr
3. request ssl

## Configure AWS loadbalancer

### PEM encode private key

1. openssl rsa -in ./domainname_com.key -outform PEM -out domainname_com.key.pem 
2. cat domainname_com.key.pem

### PEM encode certificate

1. openssl x509 -in domainname_com.crt -outform pem -out ./pem/domainname_com.crt.pem
2. cat domainname_com.key.pem

### PEM encode root certificates

1. openssl x509 -in ./Root\ Certificates/AddTrustExternalCARoot.crt -outform pem -out ./pem/AddTrustExternalCARoot.crt.pem 

2. openssl x509 -in ./Root\ Certificates/RSAAddTrustCA.crt -outform pem -out ./pem/RSAAddTrustCA.crt.pem

3. openssl x509 -in ./Root\ Certificates/RSAExtendedValidationSecureServerCA.crt -outform pem -out ./pem/RSAExtendedValidationSecureServerCA.crt.pem

### Create PEM encoded CAChain

1. cat ./pem/RSAExtendedValidationSecureServerCA.pem > ./pem/CAChain.pem
2. cat ./pem/RSAAddTrustCA.pem >> ./pem/CAChain.pem
3. cat ./pem/AddTrustExternalCARoot.pem >> ./pem/CAChain.pem

### Upload with AWS CLI or create it manualy in ACM
