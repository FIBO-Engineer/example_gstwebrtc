######################
# Reference link https://stackoverflow.com/questions/7580508/getting-chrome-to-accept-self-signed-localhost-certificate
# Steps
######################
1. Become a CA
2. Sign your certificate using your CA cert+key
3. Import myCA.pem as an "Authority" (not into "Your Certificates") in your Chrome settings (Trusted Root Certification Authorities in Manage certificates)
4. Use the $NAME.crt and $NAME.key files in your server

######################
# Become a Certificate Authority
######################

# Generate private key
openssl genrsa -des3 -out myCA.key 2048
# Generate root certificate
openssl req -x509 -new -nodes -key myCA.key -sha256 -days 825 -out myCA.pem

######################
# Create CA-signed certs
######################

NAME=mydomain.example # Use your own domain name
# Generate a private key
openssl genrsa -out $NAME.key 2048
# Create a certificate-signing request
openssl req -new -key $NAME.key -out $NAME.csr
# Create a config file for the extensions (Change IP.1 to your IP)
>$NAME.ext cat <<-EOF
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names
[alt_names]
DNS.1 = $NAME # Be sure to include the domain name here because Common Name is not so commonly honoured by itself
DNS.2 = bar.$NAME # Optionally, add additional domains (I've added a subdomain here)
IP.1 = 192.168.0.13 # Optionally, add an IP address (if the connection which you have planned requires it)
EOF
# Create the signed certificate
openssl x509 -req -in $NAME.csr -CA myCA.pem -CAkey myCA.key -CAcreateserial \
-out $NAME.crt -days 825 -sha256 -extfile $NAME.ext

# check your work to ensure that the certificate is built correctly:
openssl verify -CAfile myCA.pem -verify_hostname bar.mydomain.example mydomain.example.crt


######################
# Optional
# If you use window, you can use Docker Desktop to use openssl command
######################

# Docker command
docker volume create temp 
docker run -it -v temp:/home --name ubuntufibo ubuntu /bin/bash

# Bash shell
cd /home
# And then run openssl commands to generate all files and then get the files from 
# Folder Location : Type in the Windows file explorer :
# For Docker Engine v20.10.16: \\wsl$\docker-desktop-data\data\docker\volumes\temp