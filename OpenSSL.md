OpenSSL Usefull Snippets
========

A bit of Background
-------

We have two encodings DER and PEM (also used as extension)

DER is binary encoding and PEM is Base64 encoding

Check if a Key matches with cert
--------

    openssl pkey -in privateKey.key -pubout -outform pem | sha256sum
    openssl x509 -in certificate.crt -pubkey -noout -outform pem | sha256sum

Check if a Key matches with CRT and CSR
--------

    openssl x509 -noout -modulus -in CERTIFICATE.crt | openssl md5
    openssl req -noout -modulus -in CSR.csr | openssl md5
    openssl rsa -noout -modulus -in PRIVATEKEY.key | openssl md5

Transform .crt to .pem (encode Base64)
--------

    openssl x509 -in mycert.crt -out mycert.pem -outform PEM

Transform a (.PEM .CRT) to .PFX
--------

    openssl pkcs12 -export -in certfile.pem -inkey private_key.key -out certfile.pfx

Generate a New Private Key
--------

    openssl genrsa -out privatekey.key nopass

Generate a New Certificate Request
--------

    openssl req -new -key privatekey.key -out req.csr 
