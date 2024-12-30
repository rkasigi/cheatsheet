# CryptoKey Cheatsheet


### Extract certificate from p12 file
```
openssl pkcs12 -in INFILE.p12 -out OUTFILE.key -nodes -nocerts
```

### Extract private key from p12 file
```
openssl pkcs12 -in INFILE.p12 -out OUTFILE.crt -nokeys
```

### Convert a PEM certificate to a DER certificate
```
openssl x509 -inform pem -in Certificate.pem -outform der -out Certificate.der
```

### Convert a PEM private key to a DER private key
```
openssl rsa -inform pem -in PrivateKey.pem -outform der -out PrivateKey.der
```
