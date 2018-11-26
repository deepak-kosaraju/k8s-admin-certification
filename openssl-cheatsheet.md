# OpenSSL commands reference

## Generate Server Key

- Generate a new 2048-bit private key

```bash
sudo openssl genrsa -des3 -out `hostname -f | sed 's/\./_/g'`.key 2048
```

- Remove the password from the private key to ensure apps read the key without the need of password

```bash
sudo openssl rsa -in `hostname -f | sed 's/\./_/g'`.key -out `hostname -f | sed 's/\./_/g'`.key
```

- If the password was successfully removed, you can view the certificate contents without providing a password.

```bash
sudo openssl rsa -in `hostname -f | sed 's/\./_/g'`.key -text
```

- Create a Certificate Authority (CA) request and obtain your server certificate

```bash
sudo openssl req -new -key `hostname -f | sed 's/\./_/g'`.key -out `hostname -f | sed 's/\./_/g'`.csr -subj "/C=US/ST=CA/L=San Francisco/O=CKA Sandbox/OU=IT/CN=$(hostname -f)/emailAddress=cka@sandbox.com"
```

## Validate CSR

```bash
sudo 'openssl req -in ~/`hostname -f | sed 's/\./_/g'`.csr -noout -text 2> /dev/null' | egrep -i 'sandbox'
```
