# CryptoKey Cheatsheet


### Extract certificate from p12 file
```
openssl pkcs12 -in INFILE.p12 -out OUTFILE.key -nodes -nocerts
```

if you got an error like below, you can add `-legacy` parameter in it
```
4042C6F701000000:error:0308010C:digital envelope routines:inner_evp_generic_fetch:unsupported:crypto/evp/evp_fetch.c:355:Global default library context, Algorithm (RC2-40-CBC : 0), Properties ()
```

the complete command using `-legacy`
```
openssl pkcs12 -in INFILE.p12 -out OUTFILE.key -nodes -nocerts -legacy
```


### Extract private key from p12 file
```
openssl pkcs12 -in INFILE.p12 -out OUTFILE.crt -nokeys
```

if you got an error like below, you can add `-legacy` parameter in it
```
4042C6F701000000:error:0308010C:digital envelope routines:inner_evp_generic_fetch:unsupported:crypto/evp/evp_fetch.c:355:Global default library context, Algorithm (RC2-40-CBC : 0), Properties ()
```

the complete command using `-legacy`
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
