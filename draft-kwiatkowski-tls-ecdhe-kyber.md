---
title: Hybrid ECDHE-Kyber Key Agreement for TLSv1.3
abbrev: ECDHE-Kyber
category: info

docname: draft-kwiatkowski-tls-ecdhe-kyber-latest
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
number:
date:
stand_alone: yes
consensus: true
v: 3
ipr: trust200902
# area: AREA
workgroup: None
keyword:
 - kyber
 - post-quantum
venue:
  group: TLS
  type: Working Group
  github: https://github.com/post-quantum-cryptography/draft-kwiatkowski-tls-ecdhe-kyber
  latest: https://post-quantum-cryptography.github.io/draft-kwiatkowski-tls-ecdhe-kyber/

author:
  - ins: K. Kwiatkowski
    name: Kris Kwiatkowski
    organization: PQShield, LTD
    email: kris@amongbytes.com
  - ins: P. Kampanakis
    name: Panos Kampanakis
    organization: AWS

normative:
  kyber: I-D.cfrg-schwabe-kyber

informative:
  hybrid: I-D.ietf-tls-hybrid-design
  tlsiana: I-D.ietf-tls-rfc8447bis
  ECDSA:
       title: "Public Key Cryptography for the Financial Services Industry: The Elliptic Curve Digital Signature Algorithm (ECDSA)"
       author:
         org: American National Standards Institute
       date: 2005-11
       seriesinfo:
         ANSI: ANS X9.62-2005
--- abstract

This draft defines a hybrid key agreement for TLS 1.3 that combines
a post-quantum KEM with elliptic curve Diffie-Hellman (ECDHE).

--- middle

# Introduction

## Motivation
Kyber is a key encapsulation method (KEM) designed to be resistant to cryptanalytic attacks with quantum computers. Standardization of Kyber KEM is expected to be finalized in 2024.

Experimentation and early deployments are crucial part of the migration to post-quantum cryptography. To promote interoperability of those deployments this document provides specification of preliminary hybrid post-quantum key agreement to be used in TLS 1.3 protocol.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Negotiated Groups

This document defines additional supported groups which can be used for hybrid post-quantum key agreements. The hybrid key agreement for TLS 1.3 is detailed in the {{hybrid}} draft. We compose the hybrid scheme with the Kyber KEM as defined in {{kyber}} draft, and the ECDHE scheme parametrized with elliptic curves defined in ANSI X9.62 [ECDSA] and FIPS 186-5 {{?DSS=DOI.10.6028/NIST.FIPS.186-5}}.

## Construction

The client's share is a concatenation of the ECDHE ephemeral share and Kyber's
public key. The server's share is a concatenation of ECDHE ephemeral share and
Kyber's ciphertext returned from encapsulation (see {{kyber}}). Finally, the shared secret is a concatenation of the ECDHE shared secret and the Kyber's shared secret value returned from either encapsulation (on the server side) or decapsulation (on the client side).

## Defined Hybrid Groups

The table below lists all additional supported groups defined by this document and provides the total size (in bytes) of generated shares and calculated secret.

~~~
+---------------------+--------------+--------------+---------------|
| Hybrid Group name   | Client Share | Server Share | Shared Secret |
+---------------------+--------------+--------------+---------------|
| secp256r1_kyber768  |         1248 |         1152 |            64 |
| secp384r1_kyber768  |         1280 |         1184 |            80 |
| secp384r1_kyber1024 |         1664 |         1664 |            80 |
| secp521r1_kyber1024 |         1682 |         1682 |            98 |
+---------------------+--------------+--------------+---------------|
~~~

# Security Considerations

The same security considerations as those described in {{hybrid}} apply to the approach used by this document.
Implementers are encouraged to use implementations resistant to side-channel attacks, especially those that can be applied by remote attackers.

# IANA Considerations

This document requests/registers a new entry to the TLS Named Group
 (or Supported Group) registry, according to the procedures in
 {{Section 6 of tlsiana}}.

 Value:
 : 0x6400

 Description:
 : secp256r1_kyber768

 DTLS-OK:
 : Y

 Recommended:
 : N

 Reference:
 : This document

 Comment:
 : Pre-standards version of Kyber768


 Value:
 : 0x6401

 Description:
 : secp384r1_kyber768

 DTLS-OK:
 : Y

 Recommended:
 : N

 Reference:
 : This document

 Comment:
 : Pre-standards version of Kyber768

 Value:
 : 0x6402

 Description:
 : secp384r1_kyber1024

 DTLS-OK:
 : Y

 Recommended:
 : N

 Reference:
 : This document

 Comment:
 : Pre-standards version of Kyber1024


--- back
