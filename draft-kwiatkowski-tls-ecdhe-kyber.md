---
title: Post-quantum hybrid ECDHE-MLKEM Key Agreement for TLSv1.3
abbrev: ECDHE-MLKEM
category: info

docname: draft-kwiatkowski-tls-ecdhe-mlkem-latest
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
 - MLKEM
 - post-quantum
venue:
  group: TLS
  type: Working Group
  github: post-quantum-cryptography/draft-kwiatkowski-tls-ecdhe-mlkem
  latest: https://post-quantum-cryptography.github.io/draft-kwiatkowski-tls-ecdhe-kyber/

author:
  - ins: K. Kwiatkowski
    name: Kris Kwiatkowski
    organization: PQShield, LTD
    email: kris@amongbytes.com
  - ins: P. Kampanakis
    name: Panos Kampanakis
    organization: AWS
    email: kpanos@amazon.com

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
MLKEM is a key encapsulation method (KEM) designed to be resistant to cryptanalytic attacks with quantum computers. Standardization of MLKEM is expected to be finalized in 2024.

Experimentation and early deployments are crucial part of the migration to post-quantum cryptography. To promote interoperability of those deployments this document provides specification of preliminary hybrid post-quantum key agreement to be used in TLS 1.3 protocol.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Negotiated Groups

This document defines an additional supported group which can be used for
hybrid post-quantum key agreements. The hybrid key agreement for TLS 1.3 is
detailed in the {{hybrid}} draft. We compose the hybrid scheme with the MLKEM
as defined in {{kyber}} draft, and the ECDHE scheme parametrized with
elliptic curves defined in ANSI X9.62 [ECDSA] and NIST SP 800-186
{{?DSS=DOI.10.6028/NIST.SP.800-186}}.

The new group allows deriving TLS session keys by using FIPS-approved schemes.
NIST's special publication 800-56Cr2 {{?SP56C=DOI.10.6028/NIST.SP.800-56Cr2}}
approves the usage of HKDF {{HKDF}} with two distinct shared secrets as long as the first
one is computed by a FIPS-approved key-establishment scheme. Both ECDHE and a curve
secp256r1 (NIST P-256) are FIPS-approved by NIST SP 800-56Ar3 {{?SP56A=DOI.10.6028/NIST.SP.800-56Ar3}}
and NIST SP 800-186 {{?DSS=DOI.10.6028/NIST.SP.800-186}} correspondingly.

## Construction

The name of the new supported hybrid post-quantum group is SecP256r1MLKEM768Draft00.

When this group is negotiated, the client's share is a fixed-size concatenation of
the ECDHE share and MLKEM's public key. The ECDHE share is the serialized value of
the uncompressed ECDH point representation as defined in Section 4.2.8.2 of {{!RFC8446}}.
The MLKEM's ephemeral share is the public key of the KeyGen step (see {{kyber}}) represented
as an octet string. The size of client share is 1249 bytes (65 bytes of ECDHE part and
1184 of MLKEM part).

The server's share is a fixed-size concatenation of ECDHE share and MLKEM's ciphertext
returned from encapsulation (see {{kyber}}). The server ECDHE share is the serialized
value of the uncompressed ECDH point representation as defined in Section 4.2.8.2
of {{!RFC8446}}. The server share is the MLKEM's ciphertext returned from the Encapsulate step
(see {{kyber}}) represented as an octet string. The size of server's share is 1153 bytes (65 bytes
of ECDHE part and 1088 of MLKEM part).

Finally, the shared secret is a concatenation of the ECDHE and the MLKEM
shared secrets. The ECDHE shared secret is the x-coordinate of the ECDH
shared secret elliptic curve point represented as an octet string as
defined in Section 7.4.2 of {{!RFC8446}}. The MLKEM shared secret is the
value returned from either encapsulation (on the server side) or decapsulation
(on the client side) represented as an octet string. The size of a shared secret is 64 bytes.

# Security Considerations

The same security considerations as those described in {{hybrid}} apply to the approach used by this document.
Implementers are encouraged to use implementations resistant to side-channel attacks, especially those that can be applied by remote attackers.

# IANA Considerations

This document requests/registers a new entry to the TLS Supported Groups
 registry, according to the procedures in
 {{Section 6 of tlsiana}}. These identifiers are to be used with
 the point-in-time specified versions of MLKEM in the third round
 of NIST's Post-quantum Project which is specified in {{kyber}}.
 The identifiers used with the final, ratified by NIST, version
 of MLKEM will be specified later with in a different draft.
 \[ EDNOTE: The identifiers for the final, ratified version of
 MLKEM should preferably by different that the commonly used
 [OQS codepoints](https://github.com/open-quantum-safe/openssl/blob/OQS-OpenSSL_1_1_1-stable/oqs-template/oqs-kem-info.md) \]

 Value:
 : 25498 (0x639A)

 Description:
 : SecP256r1MLKEM768Draft00

 DTLS-OK:
 : Y

 Recommended:
 : N

 Reference:
 : This document

 Comment:
 : Combining secp256r1 ECDH with pre-standards version of MLKEM768

--- back

# Change log

> [**RFC Editor:** Please remove this section]

* draft-kwiatkowski-tls-ecdhe-mlkem-02:
  * Change Kyber name to MLKEM

* draft-kwiatkowski-tls-ecdhe-kyber-01: Fix size of key shares generated by the client and the server

* draft-kwiatkowski-tls-ecdhe-kyber-00: updates following IANA review
