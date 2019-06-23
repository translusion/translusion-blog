---
title: "Cryptography"
description: "An overview over cryptography, to get you started."
date: "2014-05-04"
categories: 
    - "Cryptography"
---

There are a lot of sources, tutorial, wiki pages and what not about cryptography, so I will not try to repeat that here. Instead this will be mainly an overview, to help you figure out what is out there. I will also try to explain concepts which I believe are not explained very well elsewhere. The focus will be on cryptography as a user. Understand enough to be able to pick the right tools and approaches.

# Symetric Encryption

Plain text is encrypted using a method or algorithm called a *cipher*.

![Cipher](/images/articles/crypto/cipher.svg)

Typically the method can be used for both encryption and decryption. With the early approaches, the cipher had to be kept secret, because anybody who knew the cipher could decrypt the secret message. So one realized that a *secret key* will have to be used, so that if the if the secret methods of information exchange was broken would would only have to replace the key and not the cipher.

![Symetric](/images/articles/crypto/symetricencryption.svg)

# Asymetric Encryption

Key distribution becomes a big problem with symetric encryption. To be on the safe side you have to distribute a different key to everyone you communicate with some they can't read each others messages. This becomes a maintenance nightmare.

It also requires a secure channel to send someone your secret encryption key. Thus *public key encryption* or *asymetric encryption* was developed. With *public key encryptin* we use two keys:

* Public key. Which is not secret. It can be shared with anyone. Think of it as a padlock.
* Private key. Should be kept secret and never shared. Think of it as the key to the padlock.

Usually there is no fundamental difference between a *public* and *private* key. It is only their usage which determines which one is public and which one is private. One can use both for encryption  and decryption.

![Asymetric](/images/articles/crypto/asymetricencryption.svg)

The image above shows the typical usage. But one could reverse it and use the private key for encryption and the public key for decryption. This is utilized for digital signatures.

# Hashes

Data is transformed using a `hash function` into something we can either call:

* A hash
* Digital fingerprint
* Message digest

![Hashing](/images/articles/crypto/hashfunc.svg)

A hash function is different from a cipher in that it only goes one way. Multiple messages may be turned into the same `digital fingerprint`. If we only need to decide whether two messages are equal we can compare just the hashes if there exists a **sufficiently large number** of hashes so that collision is practically impossible.

E.g. an attacker can't trivially create a bogus message with the same hash as the valid message unless trial and error is performed, which would be impractical if the number of possible hashes is very large.

Hashes are used with digital signatures to **compress** a message so we only have small amounts of data to encrypt and decrypt.

## Usage of Hash Functions

Hash function have a wide variety of applications, which might not be immediatly obvious:

* Store passwords safely (e.g. Unix password file)
* Create encryption keys
* Deterministic password generation
* Cheaply check over the network if two files or parts of a file are equal
* Create a fingerprint for a public key

The properties of hashes that make them usefull for all these tasks is that when hashing something you lose information, so it is impossible to recreate the original message from a hash. This is used in a Unix login system to store passwords as hashes. A login is then performed by hashing the users provided password and comparing it to a hashed password in the password file. This way if a hacker is able to get hold of the password file there is no way to decrypt the file and see all the passwords in plaintext. He or she has to find the password through brute force.

You can use this property to combine simple passwords with other information to create strong passwords. E.g. if you concatinate "pass123" and "amazon" as you login password for amazon this will produce a string full of giberish. 

### Different algorithms for different Usage

The desired properties of the hashing function used will vary depending on application. If you want to compare two files on separat locations on a network you want to be able to hash the content of the files **fast**, before you transmit the hash to compare the files.

But if you derive a password or a key using a hash you probably want it to be **slow**, because you want a brute force attack to be time consuming.


# Digital Signatures

The sender sends a message to a receiver in plain text. But the sender attached a *digital signature* to the message to prove he/she wrote the message.

### Sender
To accomplish this, the message is (1) hashed to a digest (hash). The digest is (2) encrypted using the senders `private key`. The resulting *digital signature* could only have been created by someone knowing the `private key`. This signature is attached to the message and sent to the receiver. 

![Digital Signature](/images/articles/crypto/digitalsigning.svg)

### Receiver
The receiver decrypts (3) the signature using the sendes *public key*. The resulting digest is compared (4) to the digest the receiver computes by hashing the received message. If they are equal, it is proof that the creator of the message could only have been someone knowing the corresponding *private key*.
