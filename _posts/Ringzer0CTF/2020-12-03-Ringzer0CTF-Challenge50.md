---
layout: post
title: Ringzer0CTF Public key recovery
categories: [Write-Ups, Ringzer0CTF, Cryptography]
tags: [Ringzer0CTF, Cryptography, public key, MD5]
featured-image:  ringzer0CTF/icon.png
featured-image-alt: ringzer0CTF
---

It's a write-up about the challenge : [Ringzer0CTF - Public key recovery](https://ringzer0ctf.com/challenges/49)

# Challenge 50 - Public key recovery

## 1. Challenge 

 -----BEGIN RSA PRIVATE KEY-----
MIICXgIBAAKBgQDwkrxVrZ+KCl1cX27SHDI7EfgnFJZ0qTHUD6uEeSoZsiVkcu0/
XOPbz1RtpK7xxpKMSnH6uDc5On1IEw3A127wW4Y3Lqqwcuhgypd3Sf/bH3z4tC25
eqr5gA1sCwSaEw+yBxdnElBNOXxOQsST7aZGDyIUtmpouI1IXqxjrDx2SQIDAQAB
AoGBAOwd6PFitnpiz90w4XEhMX/elCOvRjh8M6bCNoKP9W1A9whO8GJHRnDgXio6
/2XXktBU5OfCVJk7uei6or4J9BvXRxQpn1GvOYRwwQa9E54GS0Yu1XxTPtnBlqKZ
KRbmVNpv7eZyZfYG+V+/f53cgu6M4U3SE+9VTlggfZ8iSqGBAkEA/XvFz7Nb7mIC
qzQpNmpKeN4PBVRJBXqHTj0FcqQ5POZTX6scgE3LrxVKSICmm6ungenPXQrdEQ27
yNQsfASFGQJBAPL2JsjakvTVUIe2JyP99CxF5WuK2e0y6N2sU3n9t0lde9DRFs1r
mhbIyIGZ0fIkuwZSOqVGb0K4W1KWypCd8LECQQCRKIIc8R9iIepZVGONb8z57mA3
sw6l/obhfPxTrEvC3js8e+a0atiLiOujHVlLqD8inFxNcd0q2OyCk05uLsBxAkEA
vWkRC3z7HExAn8xt7y1Ickt7c7+n7bfGuyphWbVmcpeis0SOVk8QrbqSNhdJCVGB
TIhGmBq1GnrHFzffa6b1wQJAR7d8hFRtp7uFx5GFFEpFIJvs/SlnXPvOIBmzBvjU
yGglag8za2A8ArHZwA1jXcFPawuJEmeZWo+5/MWp0j+yzQ==
-----END RSA PRIVATE KEY-----


Public key = md5(base64 blob only) MD5 2 first bytes are 0x42f5 


## 2. Solution

 - Google: how recover public key from private key openssl
	found https://stackoverflow.com/questions/5244129/use-rsa-private-key-to-generate-public-key

We used openssl to recover the public key from the RSA private key.

```openssl rsa -in private.pem -pubout > public.pem```


We copy the public key in a file : key.txt and we used the command.

```
md5 key.txt
MD5 (public.pem) = 42f51df8b6a2bafa824c179a38066e5d
```

The 2 first bytes are 0x42f5, so we submited this md5 to find the flag :

```FLAG-9869O2dQ43d1r116kfD0Sj5n```