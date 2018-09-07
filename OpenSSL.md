OpenSSL Usefull Snippets
========

Check if a Key matches with cert
--------

    openssl pkey -in privateKey.key -pubout -outform pem | sha256sum
    openssl x509 -in certificate.crt -pubkey -noout -outform pem | sha256sum
