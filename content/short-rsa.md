+++
title = "A short description of RSA-encryption"
date = 2020-10-17

[taxonomies]
tags = ["Cryptography"]
+++
The RSA cryptosystem is an asymmetric encryption/decryption scheme. It being asymmetric means, to put it simply, that the decryption is not a "reversed" encryption that can be worked out by knowing how information is encrypted. <!-- more -->An example of a symmetric scheme would be that in order to encrypt some number, 3 is added to it; to decrypt it, 3 is simply subtracted. Let’s say you want to decrypt the number 5, then the encrypted version of it (known as the ciphertext) is

5 + 3 = 8

which is decrypted by

8 – 3 = 5

The drawbacks of a symmetric encryption/decryption is that the holders of the encryption-key (3, in the example above) can also decrypt the ciphertext by turning the encryption-key 3 into -3. This could be an issue if an entity wants to receive sensitive information and asks potential sources to encrypt the information in order to not reveal their identity. If the encrypted information is leaked and the manner of encrypting messages is publicly known, all the leaked information can be decrypted and the sources identity can be revealed.

RSA encrypts and decrypts information using different keys, meaning that knowing how to encrypt the messages is not enough to be able to decrypt the ciphertext.

c = m<sup>d</sup> mod n

m = c<sup>e</sup> mod n

Where “mod” signifies “modulus”, which calculates the remainder when dividing the number to the left of the operator with that to the right. That is, 11 mod 3 = 2, since 11 – 2 = 9, which is divisible by 3.

An example of using RSA key-pair key 1 = (77, 37) and key 2 = (77, 13) to encrypt the number 5:

c = 5<sup>37</sup> mod 77 = 47

47, being the result of encrypting 5 with key 1, can then be decrypted with key 2:

m = 47<sup>13</sup> mod 77 = 5

which returns the number that was encrypted with key 1. A useful property is that key 1 can also be used to decrypt numbers encrypted with key 2. Encrypting 5 with key 2, we get:

c = 5<sup>13</sup> mod 77 = 26

which when decrypted with key 1gives us

m = 26<sup>37</sup> mod 77 = 5

A common usage of RSA is proving the identity of certain party. If I have published one of these keys, known as the public key, I can prove to be holding the other secret key, the private key, by encrypting information that can be decrypted by the public key or vice versa. Of course, this is not bullet-proof, since the private key could be stolen.

The manner in which which to generate the keys is quite straight-forward, although it can become very technical when aiming for high speed and safety, changing the order of some steps:

1. Generate to prime numbers, `p` and `q`. This can be done using the the Sieve of Eratosthenes algorithm.

2. Calculate `n = pq`.

3. Calculate `lambda = (p – 1)(q – 1)`.

4. Choose a number `e` which is `1 < e < lambda` and which is co-prime with `lambda`. If a list of prime numbers has been generated in step 1, one of these can be chosen that matches the first requirement.

5. Generate a number `d`, which satisfies `de ≡ 1 (mod lambda)`, i.e. `d = (1 + x lambda) / e`. This is best calculated by using a suitable algorithm (such as the Extended Euclidean algorithm), and not by simply choosing an arbitrary `x`, seeing if it satisfies the condition, and if not, incrementing it and repeating the process until it does.

Even if it’s unfeasible to reverse the encryption scheme, it’s possible for an attacker to create a table of inputs and their corresponding encrypted outputs and then using it to try and determine what a user’s input was. Let’s say you’ve written a simple RSA-enryption software, where each letter is converted to a number and then encrypted like this:

hello → (convert) → 8 5 12 12 15 → (encryption) → 412 846 142 142 384

The attacker doesn’t have to completely know what happens in each step: it’s sufficient if they create a table where all of the inputs and corresponding outputs are stored, such as:

A → 912

B → 124

C → 43

D → 613

E → 846

...

Z → 499

With this table, the attacker can use it as a dictionary to look up what letter each number corresponds to. To encrypt in a manner that protects against this, it's better to encrypt in larger blocks, like 20 symbols at a time instead of 1. Then it won't be enough to have a table of letters and their corresponding output since words like "duck" and "deck" won't have outputs that seem related. This should also be complemented with so called padding, where information is added at different parts of the message that is to be encrypted in order to further obscure the connection between the input and the encrypted output. Upon decryption, this added information can be removed.

Cryptography is a very complex subject, and I think this is a suitable point to stop if what you’re after is to have a basic overreaching understanding of the most important aspects of RSA.
