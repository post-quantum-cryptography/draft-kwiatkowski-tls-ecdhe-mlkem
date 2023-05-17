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
  HKDF: DOI.10.17487/RFC5869
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

This document defines an additional supported group which can be used for
hybrid post-quantum key agreements. The hybrid key agreement for TLS 1.3 is
detailed in the {{hybrid}} draft. We compose the hybrid scheme with the Kyber
KEM as defined in {{kyber}} draft, and the ECDHE scheme parametrized with
elliptic curves defined in ANSI X9.62 [ECDSA] and NIST SP 800-186
{{?DSS=DOI.10.6028/NIST.SP.800-186}}.

The new group allows deriving TLS session keys by using FIPS-approved schemes.
NIST's special publication 800-56Cr2 {{?SP56C=DOI.10.6028/NIST.SP.800-56Cr2}}
approves the usage of HKDF {{HKDF}} with two distinct shared secrets as long as the first
one is computed by a FIPS-approved key-establishment scheme. Both ECDHE and a curve
secp256r1 (NIST P-256) are FIPS-approved by NIST SP 800-56Ar3 {{?SP56A=DOI.10.6028/NIST.SP.800-56Ar3}}
and NIST SP 800-186 {{?DSS=DOI.10.6028/NIST.SP.800-186}} correspondingly.

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
+---------------------+--------------+--------------+---------------|
~~~

# Security Considerations

The same security considerations as those described in {{hybrid}} apply to the approach used by this document.
Implementers are encouraged to use implementations resistant to side-channel attacks, especially those that can be applied by remote attackers.

# IANA Considerations

This document requests/registers a new entry to the TLS Named Group
 (or Supported Group) registry, according to the procedures in
 {{Section 6 of tlsiana}}. These identifiers are to be used with
 the point-in-time specified versions of Kyber in the third round
 of NIST's Post-quantum Project which is specified in {{kyber}}.
 The identifiers used with the final, ratified by NIST, version
 of Kyber will be specified later with in a different draft.
 \[ EDNOTE: The identifiers for the final, ratified version of
 Kyber should preferrably by different that the commonly used
 [OQS codepoints](https://github.com/open-quantum-safe/openssl/blob/OQS-OpenSSL_1_1_1-stable/oqs-template/oqs-kem-info.md) \]

 Value:
 : 0x6400

 Description:
 : secp256r1_kyber768round3_d00

 DTLS-OK:
 : Y

 Recommended:
 : N

 Reference:
 : This document

 Comment:
 : Combining secp256r1 ECDH with pre-standards version of Kyber768 (NIST Round 3)

--- back
