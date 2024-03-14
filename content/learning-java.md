+++
title = "Learning the basics of Java by writing an RSA-app"
date = "2021-01-21"

[taxonomies]
tags = ["Java", "Cryptography"]
+++

Due to the similarities of Python and Java, and Java being so widely used, I wanted to learn at least the basics of the language. I recently took a university course in Java, where one part of it consisted in writing an application which had to meet certain criteria of complexity – other than that, you were free to choose the project yourself. Below, I’ll briefly discuss the RSA-encryption app I wrote and what my impression of Java is. RSA-encryption is explained in this [post](/short-rsa).
<!-- more -->

# The application
## Menu

The menu gives you the option to generate your own keys or supplying your own keys (in the latter case, tests are run to verify that the keys are correct). In the next stage, you are given the option to encrypt, decrypt or view the keys. In each stage you can also access a simple description of the application.

## Generating keys

The generation of the keys follows the steps presented in my previous post, except for one difference: `e` is set to 65537 in order to make encryption more effective. `d` is calculated by a function already present in Java (`d` is the so called modular inverse of `e`).

## Transforming with maps

In order to encrypt non-numerical symbols, I chose to transform the symbols of the input message using a simple map where each symbol is mapped to a certain number (starting from 2, since 0<sup>x</sup> = 0 and 1<sup>x</sup> = 1), differing by 1 to its closest neighbor.

## Large integers

One requirement for using RSA securely is to use large integers as part of the public and private keys. This causes issues in Java if not handled with extra care, since the largest number the type integer can contain is 2<sup>31</sup>-1 = 2147483647, which can quickly be surpassed during encryption and decryption. This can be avoided by using the `BigInteger` class or by taking advantage of the following mathematical relationship:

```
(a b) mod n = ((a mod n) (b mod n)) mod n
```

This means that

```
a³ mod n = (a a a) mod n = ((a mod n) (a mod n) (a mod n)) mod n = (a² mod n) (a mod n)) mod n
```

which keeps the handled numbers from becoming ≥n<sup>2</sup>. In the end, I chose to both use `BigInteger` to get familiar with the type class and to also implement a solution using the relationship presented above as practice, even though this ought to have slightly detrimental effects on speed.
What the app lacks

I chose to encrypt messages symbol by symbol which is not secure. It also lacks any kind of padding. The disadvantages of these are also briefly discussed in my previous post.

# My impression of Java
## Static typing

Coming from Python, something that stood out for me right away was the static typing. I found this annoying at first, but gradually it seemed like something that ensures that safer code is written, and underscored why it is good practice to explicitly type (or state the type of) objects/variables in Python.

## Dated and verbose

Coming from Python, a lot of the solutions to problems in Java felt a bit convoluted without any obvious advantages (like higher control of memory). The same goes for it’s verbose style, save for the curly brackets used for wrapping blocks. They can be done away with in a programming language of course, like in Python where only indentation is used, but I feel like a bit of readability is gained with them.

## Conclusions

Java feels a bit dated. Due to its large amount of boilerplate code and convoluted manner of achieving certain objectives (at least in comparison with Python but without the advantages of a low level language), it's not something that I'd like to work professionally with nor develop projects in. What I do take away from it though is the advantages of static typing, underscoring the importance of certain practices of coding in Python and increasing my interest of languages that use static typing, and giving me a more positive view of languages that are a tiny bit more verbose than Python (using {} in place of only indentation, for example).

# Source

The code for the application can be found [here](https://github.com/vcrn/rsa-app-java).
