# Encrypting Data Securely

## Symmetric v Asymmetric
* Symmetric data
    * is one where the same secret key is used to encrypt and decrypt data.
    * There is only one.
* Asymmetric is where there are 2 keys. 
    * One only for encryption which is public
    * another for decryption, and is private
    * The public keys are mathmatically derived from the private keys.
    * Different services can encrypt. Only one can decrypt.

## What is "Strong" Encyrption?
1. Confidentiality—Ensuring that the data cannot be read except by an authorized party,
2. Integrity—Ensuring that the data has not been tampered with between when it was written and when it is read
3. Authenticity—Ensuring that the data comes from the expected source.
* At flight, most systems will use VPCs and HTTPS over TLS to ensure data is secure.

## The AES Algorithm
* An industry/gov standard for encryption in 2021.
* [Here are the modes this algo can run in. Each has their pros & cons](https://www.highgo.ca/2019/08/08/the-difference-in-five-modes-in-the-aes-encryption-algorithm/)
* GCM (Galois/Counter Mode) is a well-rounded choice. It also provides:
    * Ciphertext indistinguishability: Encrypting & reencrypting data produces random results, if random "initialization vector" (IV) are used. Makes encrypted data indistiguishable from random data (like salting a pass).
    * Integrity: Any attempt to tamper with the data will cause decryption to fail.
    * Do you want more Integrity?
        * Additional Authenticated Data - assert that certain data belongs to certain user
        * [Authenticated Encryption with Additional Data](https://en.wikipedia.org/wiki/Authenticated_encryption#Authenticated_encryption_with_associated_data_(AEAD)) 
        