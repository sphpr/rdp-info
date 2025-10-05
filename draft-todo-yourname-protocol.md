---
title: "A Historical Analysis of Design Lessons from The Reliable Data Protocol Version 1 and Version 2"
abbrev: "RDP-History"
category: info

docname: draft-todo-yourname-protocol-latest
submissiontype: independent  # also: "independent", "editorial", "IAB", or "IRTF"
v: 3
area: Transport
keyword: Reliable Data Protocol

author:
 -
    fullname: Sophie Harper
    organization: Independent
    email: sphpr@proton.me

normative:
  RFC908:
  RFC1151:

informative:
  RFC2018:
  Partridge87:
       title: "Private Communication"
       author:
         name: Partridge, C.
       date: February 1987

...

--- abstract

This document reviews and analyzes the history and experience of the Reliable Data Protocol (RDP). RDP was initially published as an experimental protocol in RFC 908 in 1984 and subsequently revised in RFC 1151 in 19901. RDP aimed to provide a message-oriented, reliable transport service to meet specific application requirements such as remote loading. This document summarizes the key technical experiences gained from RDP's experimental deployment2, details the lessons learned regarding checksum performance and flow control mechanisms, and assesses the conceptual influence of its design principles on subsequent IETF transport protocols (e.g., TCP SACK and QUIC). The document is intended for historical archiving, providing a reference for early explorations of Internet transport protocols, and does not propose any new standards or deployment recommendations.

--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
