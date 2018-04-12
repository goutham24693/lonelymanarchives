---
layout: post
title:  "Code Signing and Verification"
date:   2018-04-04 23:24:40 +0530
categories: jekyll update
---

<h3>What is Code Signing?</h3>
Code signing is the process of digitally signing a software to verify the author and guarantee that the software has not been modified or corrupted since it was signed. Because of the potential damage that software/executable can cause to a computer system, it is important that users be able to trust code published on the network.
 
The process uses a cryptographic hash to validate authenticity and integrity. Cryptographic Hash Functions takes an input data and returns a fixed-size <em>digest</em>. Signing implementation will also require a set of asymmetrical key, which comprises of a public key and a private key. Public keys may be distributed widely, and private keys which are known only to the owner. If a message if encrypted using a public key, then it can be decrypted only by the matching private key.

<h3>Process of Signing and Verifying</h3>
Signing requires execution of a cryptographic hash function on the software/code to produce a <em>digest</em>. The digest is signed with author's private key to produce a <em>signature</em> and this signature is sent to the users along with the software. The user again produces the digest from the software using the same hash function, and then uses the author's public key to decrypt the <em>signature</em>. If both digests match, then the user can be sure that the code has not been tampered with.

<h3>Example signing using openssl command line tools:</h3>
Openssl is a general-purpose cryptography library that is free to use. It includes command line tools that can be used to build PKI, encrypt, decrypt and do other cryptographic operations.
<h4>Generating Key-pair:</h4>
 Creation of assymetric public-private key pair can be done using openssl command.
{% highlight bash %}
$ openssl genrsa -out priv.key 2048
  Generating RSA private key, 2048 bit long modulus
  ...................................................+++
  ...+++
  e is 65537 (0x10001)

$ openssl rsa -in priv.key -pubout -out pub.key
 writing RSA key
{% endhighlight %}
<h4>Signing using private key:</h4>
Now the software file is given as input to a cryptographic hash function to produce a fixed-length digest. This digest is then signed by the private key of the author.
{% highlight bash %}
$ openssl dgst -sha256 -sign priv.key -out file.sha256 file.txt
$ openssl enc -base64 -in file.sha256 -out file.sha256.base64
{% endhighlight %}

In the above command <em>SHA256</em> hash function is applied to the <em>file.txt</em> and the digest is signed using <em>priv.key</em>. After signing, the output is further <em>base64</em> encoded to convert the signed content to ASCII string format.

<h4>Verification using public Key:</h4>
The verifier has to compute the digest of the software using the same hash function used by the author. Then integrity and authenticity can be ensured by decrypting the signature and verifying it with the hash.
{% highlight bash %}
$ openssl enc -base64 -d -in file.sha256.base64 -out file.sha256.verify
$ openssl dgst -sha256 -verify pub.key -signature file.sha256.verify file.txt
{% endhighlight %}
<b>Note:</b> Instead of Base64 encoding technique, other strong encryption mechanism can be used (execute <em>"man enc"</em> in terminal for more). Base64 encoding simply converts all the characters to ASCII format and decoding does the opposite. Stronger algorithms like AES, DES etc can also be used for encryption. 

In case a directory needs to be signed and shared, it can be compressed into a single file and signed. After verifying the integrity, the compressed file can be extracted. Code signing are commonly used in update mechanisms, only signed packages are patched to verify the authenticity of the product.

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
