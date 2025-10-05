---
title: "A Historical Analysis of Design Lessons from The Reliable Data Protocol Version 1 and Version 2"
abbrev: "RDP-History"
category: info

docname: draft-harper-rdp-analysis-latest
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
         name: C. Partridge
       date: February 1987

...

--- abstract

This document reviews and analyzes the history and experience of the Reliable Data Protocol. RDP was initially published as an experimental protocol in RFC 908 in 1984 and subsequently revised in RFC 1151 in 1991. RDP aimed to provide a message-oriented, reliable transport service to meet specific application requirements such as remote loading. This document summarizes the key technical experiences gained from RDP's experimental deployment, details the lessons learned regarding checksum performance and flow control mechanisms, and assesses the conceptual influence of its design principles on subsequent IETF transport protocols. The document is intended for historical archiving, providing a reference for early explorations of Internet transport protocols, and does not propose any new standards or deployment recommendations.

--- middle

# Introduction

## Context and Objective

The Reliable Data Protocol (RDP) was released in 1984 via RFC 908, forming part of a series of early Internet experimental protocols. The protocol attempted to offer a transport layer service distinct from the existing Transmission Control Protocol (TCP), designed specifically for applications that require explicit message boundaries and are sensitive to latency, such as remote debugging and file transfer. Although RDP's initial version (RFC 908) was designated as experimental, and the protocol never progressed to an advanced stage of the IETF standardization process, the lessons learned from its experimental deployment hold lasting value for the Internet community. This document serves to formally record this history and to codify its technical lessons5.

## Document Scope and Methodology

This document is intended to serve as an Informational memo to provide historical context for transport protocol designers. This analysis focuses on the original RDP specification (RFC 908) and its revision (RFC 1151). Key areas of experience examined are: first, the innovative design of message orientation and selective acknowledgment; second, the performance bottlenecks and solutions for the checksum algorithm; and third, the mechanism deficiencies found in the window-based flow control. This document does not advocate for the redeployment or standardization of RDP, but rather focuses on summarizing its historical contribution through the distillation of engineering lessons.

# RDP Design Philosophy and Early Innovation

## Core Features: Message-Orientation and Optional Ordering

The core of RDP's design was to provide a message-oriented reliable service, contrasting sharply with TCP's byte-stream model. This was achieved by explicitly defining message boundaries within the protocol segments, aiming to simplify application-layer parsing and processing logic. While the specific mechanism (such as a flag or length field) is detailed in the specifications, the intent was to offer a more natural fit for discrete data transfers. Furthermore, RDP allowed applications to choose whether ordered delivery was required. This early optimization philosophy permitted the protocol to avoid unnecessary buffering and latency in scenarios where strict sequencing was not mandated.

## Pioneering Selective Acknowledgement

A key innovation of RDP was its adoption of an Explicit Acknowledgement (EACK) segment mechanism. This mechanism enabled the receiver to precisely inform the sender which specific out-of-order segments had been successfully received. This significantly reduced the unnecessary retransmission of all subsequent received segments caused by the loss of a single segment, which enhanced efficiency, particularly for bulk data transfer. The EACK mechanism represented an early, practical exploration of the concept of selective acknowledgment. However, the implementation of this mechanism also increased the complexity of state maintenance for out-of-order segments at the receiver.

# Technical Experience and Key Lessons

## Checksum Performance and Engineering Lesson

The first significant engineering challenge RDP encountered during experimental deployment was the performance of its original checksum algorithm. RFC 1151 notes that the checksum used in RDP V1 exhibited surprisingly variable computational costs across different host architectures and implementations10. In some cases, performance of optimized implementations differed by a factor of five on comparable hardware bases, severely impacting the protocol's throughput.

The variability in performance was deemed undesirable. Consequently, the revision in RDP V2 (1990) ultimately adopted the 16-bit one's complement sum algorithm used by TCP and UDP.

Lesson Learned: Performance measurement demonstrated that choosing an algorithm that is simple to implement and widely adopted can mitigate the unpredictable performance penalties associated with complex computation across varied CPU architectures, thus prioritizing engineering consistency over theoretical novelty.

## Deficiencies in Flow Control Mechanism

RDP's window-based flow control mechanism revealed a serious design flaw. RFC 1151 explicitly states that RDP's flow control scheme does not allow the receiver to close the sender's window15. Specifically, if the receiver acknowledged segments immediately upon reception, even if its buffer was full, the sender would continue to transmit data based on the acknowledgment, potentially exceeding the receiver's processing capacity.

Technical Root Cause and Lesson: The practical consequence of this flaw was receiver buffer overflow and resource exhaustion. This RDP experience underscored that in a receiver-driven flow control mechanism, precise semantics for window withdrawal (reducing the window size) are fundamental requirements for protocol stability. The inability to communicate a zero or reduced window reliably allows the sender to overrun the receiver's resources, which can lead to protocol stack instability. The suggested solution in RFC 1151 (to only acknowledge segments after delivery to the application) was explicitly noted as untested at the time of publication.

## Absence of Congestion Control

The limitations of RDP were compounded by its lack of a dedicated congestion control mechanism. While flow control is designed to protect the receiver's buffer resources, congestion control is necessary to protect the shared network resources. RDP's design primarily focused on receiver limitations, characteristic of early protocols developed before the critical need for global network congestion management was fully recognized. This systemic absence was a significant factor that precluded its large-scale adoption in the shared Internet environment.

# Conceptual Impact on Successor Protocols

Although RDP did not achieve standardization, its core innovative concepts found clear conceptual resonance in subsequent transport protocol efforts.

## Conceptual Precursor to SACK

RDP's EACK mechanism stands as one of the earliest implemented examples of selective acknowledgment technology within the IETF ecosystem. This practical experience provided a valuable reference for the subsequent evolution of the TCP protocol. The historical work of RDP demonstrated the efficiency gains offered by this mechanism in reliable transmission. This early experience conceptually foreshadowed the later SACK extension for TCP (e.g., RFC 2018) and the design of protocols like SCTP and QUIC.

## Validation for Multi-Streaming and Unordered Delivery

RDP's design, which included support for explicit message boundaries and optional ordered delivery, conceptually aligns with the later multi-streaming and unordered delivery features found in protocols such as SCTP and QUIC. RDP's experience validated the application layer's practical need for these flexible transport semantics, guiding the design direction of subsequent transport protocols intended for complex application environments.

# Summary of Engineering Lessons Learned

The design, revision, and experimental experience of RDP yield the following engineering principles for the IETF community:

Engineering Consistency Precedes Theoretical Innovation: The selection of a checksum algorithm should prioritize consistent performance and engineering simplicity across diverse hardware and implementations over theoretically marginal gains in error detection capability.

Flow Control Mechanisms Must Be Complete: It is essential to ensure that the receiver possesses a reliable and unambiguous mechanism to completely withdraw or limit the sender's transmission window, preventing receiver resource exhaustion. Flow control flaws can lead more directly to protocol stack failure than congestion control deficiencies.

Complex Features Require Cost Assessment: Although features like selective acknowledgment can enhance efficiency, the design must fully consider the implementation cost associated with receiver state maintenance and segment encoding complexity to ensure practical viability in deployment.

# Security Considerations

RFC 1151 explicitly states that Security issues are not addressed in this memo18. RDP was designed in the mid-1980s, a period when Internet protocol design generally did not prioritize authentication, encryption, or message integrity. Consequently, RDP lacks the security mechanisms required by modern protocols.

As a historical record, this document makes no security proposals. Should RDP be considered for deployment in a modern network environment (which is not recommended), a comprehensive analysis of modern security requirements would be mandatory, including message authentication, integrity protection, resistance to replay attacks, and interoperability with established security frameworks such as IPsec or TLS/DTLS.

# Conclusion

The Reliable Data Protocol (RDP) represents a valuable experimental attempt in the evolution of Internet transport protocols. It embodies early consideration of TCP's limitations, demonstrating foresight particularly in its message-oriented approach and use of selective acknowledgment. Concurrently, the flaws exposed in its checksum performance and flow control mechanisms provide an important historical case study for reinforcing rigor in subsequent IETF transport layer protocol design. This document completes the summary and archival of RDP's experience, intending to preserve this important historical reference for the Internet community.

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

The authors wish to formally acknowledge the significant contributions of the original designers and implementers of the Reliable Data Protocol (RDP). Specifically, we recognize David Velten, Robert Hinden, and Jim Sax, who authored the initial experimental specification, RFC 908. Their work represented a pioneering effort in transport protocol design, laying the groundwork for a message-oriented service in the nascent Internet.

We extend particular gratitude to Craig Partridge and Robert Hinden, who diligently revisited the protocol, summarized the critical findings from its experimental deployment, and authored RFC 1151 in 1990. This revision, which cataloged essential technical lessons concerning checksum performance and flow control deficiencies, provided invaluable, early engineering guidance to the wider Internet community.

Furthermore, acknowledgment is due to the members of the End-to-End Research Group who participated in RDP's testing and offered insights, including the proposed solution for the flow control problem.

Their collective efforts, even in the context of an experimental protocol that did not reach full standardization, continue to inform and influence the design of modern, more sophisticated transport layer technologies.
