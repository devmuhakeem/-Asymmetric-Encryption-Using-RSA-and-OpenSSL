# Asymmetric Encryption Using RSA and OpenSSL

A hands-on lab where I generated an RSA key pair from scratch and used it to encrypt and decrypt a file with OpenSSL, working entirely from the Linux command line.

## Overview
RSA is one of the most widely used asymmetric encryption algorithms, relying on a mathematically linked public/private key pair instead of a single shared key. This lab walks through the full cycle: generating the keys, encrypting data with the public key, and decrypting it with the private key.

## What I did

### 1. Generated an RSA private key
```
openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048
```
Created a 2048-bit RSA private key, a standard key size that balances security and performance.

### 2. Extracted the public key
```
openssl rsa -pubout -in private_key.pem -out public_key.pem
```
Derived the public key directly from the private key file — the two are mathematically linked, but the private key can never be derived back from the public one.

### 3. Created a test file and encrypted it
```
echo "This is a test file for RSA encryption." > test_file.txt
openssl pkeyutl -encrypt -in test_file.txt -pubin -inkey public_key.pem -out test_file_encrypted.bin
```
Encrypted the file using the public key. Viewing the output confirmed it was unreadable binary data.

### 4. Decrypted the file with the private key
```
openssl pkeyutl -decrypt -in test_file_encrypted.bin -inkey private_key.pem -out test_file_decrypted.bin
```
Decrypted the file back to its original readable text, using only the private key — confirming the public key alone could never have reversed the encryption.

## Key takeaways
- Asymmetric encryption's core property is directional: what the public key locks, only the matching private key can unlock — that's what makes it possible to share the public key openly without risk
- OpenSSL's `pkeyutl` handles both encryption and decryption through the same interface, just swapping which key and which flag (`-pubin` vs plain private key) you use
- This is the same underlying mechanism behind things like HTTPS certificate exchange and SSH key authentication, just done manually and explicitly here

## Tools
OpenSSL, Linux command line

---
*Completed as a hands-on IBM lab.*
