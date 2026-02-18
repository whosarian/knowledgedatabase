# Certificates

!!! tip "Symmetrical vs. Asymmetrical"

| Symmetrical | Asymmetrical |
| ----------- | ------------ |
| 1 key for de- and encryption | Key pair |
| very fast | Public and private key |

## Chain of trust

!!! note "A Certificate is only valid, if it has been signed by someone the computer trusts"

1. Root CA: Certificates that are already installed in the OS or browser
2. Intermediate CA: Root CAs delegate to intermediates
3. Server Certificate (Leafs): Own Certificate

## Content of a certificate

- CN (Common Name)
- SAN (Subject Alternative Name): Includes all DNS-names and IP's
- Validity
- Key Usage: Tells what the certificate is used for (Server Auth, Client Auth)

## Useful commands

Read certificate:

```sh
openssl x509 -in cert.crt -text -noout
```

Test live connection:

```sh
openssl s_client -connect elastic-node:9200 -showcerts
```

Check if key and cert belong together:

```sh
openssl x509 -noout -modulus -in server.crt | openssl md5
openssl rsa -noout -modulus -in server.key | openssl md5
```
