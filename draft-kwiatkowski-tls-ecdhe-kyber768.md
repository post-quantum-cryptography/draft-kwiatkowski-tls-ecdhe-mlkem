---
title: Hybrid ECDHE-Kyber Key Exchange for TLS
abbrev: ECDHE-Kyber768 Key Exchange
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

This draft specifies a TLS key exchange that combines the post-quantum Kyber KEM with a with
elliptic curve Diffie-Hellman (ECDHE) key exchange based on eliptic curves P-256, P-384 and P-521.

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

TODO Security



# IANA Considerations

This document requests/registers a new entry to the TLS Named Group
 (or Supported Group) registry, according to the procedures in
 {{Section 6 of tlsiana}}.

 Value:
 : 0x6400

 Description:
 : Kyber768-ECDHE-P256

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
 : Kyber768-ECDHE-P384

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
 : Kyber768-ECDHE-P521

 DTLS-OK:
 : Y

 Recommended:
 : N

 Reference:
 : This document

 Comment:
 : Pre-standards version of Kyber768




--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
