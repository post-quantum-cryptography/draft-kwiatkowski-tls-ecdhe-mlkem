---
title: Hybrid ECDHE-Kyber Key Exchange for TLSv1.3
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
  github: https://github.com/post-quantum-cryptography/draft-kwiatkowski-tls-ecdh-kyber
  latest: https://post-quantum-cryptography.github.io/draft-kwiatkowski-tls-ecdh-kyber/

author:
 -
    fullname: Kris Kwiatkowski
    organization: PQShield, LTD
    email: kris@amongbytes.com

normative:
  kyber: I-D.cfrg-schwabe-kyber

informative:
  hybrid: I-D.ietf-tls-hybrid-design
  tlsiana: I-D.ietf-tls-rfc8447bis
  KyberV302:
    target: https://pq-crystals.org/kyber/data/kyber-specification-round3-20210804.pdf
    title: CRYSTALS-Kyber, Algorithm Specification And Supporting Documentation (version 3.02)
    author:
      -
        ins: R. Avanzi
      -
        ins: J. Bos
      -
        ins: L. Ducas
      -
        ins: E. Kiltz
      -
        ins: T. Lepoint
      -
        ins: V. Lyubashevsky
      -
        ins: J. Schanck
      -
        ins: P. Schwabe
      -
        ins: G. Seiler
      -
        ins: D. Stehle # TODO unicode in references
    date: 2021
    format:
      PDF: https://pq-crystals.org/kyber/data/kyber-specification-round3-20210804.pdf


--- abstract

This draft requests codepoints for hybrid key exchange in TLS 1.3 {{hybrid}} that combines the post-quantum Kyber KEM {{kyber}} with a
elliptic curve Diffie-Hellman (ECDHE) key exchange based on eliptic curves secp256r1 (NIST P-256) and secp384r1 (NIST P-384).

--- middle

# Introduction

## Motivation

The final draft for Kyber is expected in 2024.
There are already early deployments of post-quantum key agreement,
    with more to come before Kyber is standardised.
To promote interoperability of early implementations,
    this document specifies a preliminary hybrid post-quantum key agreement.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

TODO


# IANA Considerations

This document requests/registers a new entry to the TLS Named Group
 (or Supported Group) registry, according to the procedures in
 {{Section 6 of tlsiana}}.

 Value:
 : 0x6400

 Description:
 : secp256r1-Kyber768

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
 : secp384r1-Kyber768

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
 : secp384r1-Kyber1024

 DTLS-OK:
 : Y

 Recommended:
 : N

 Reference:
 : This document

 Comment:
 : Pre-standards version of Kyber1024




--- back
