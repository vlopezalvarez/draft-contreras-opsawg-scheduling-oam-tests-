---
title: "A YANG Data Model for Network Diagnosis by scheduling sequences of OAM tests"
abbrev: "Network OAM sequences"
category: info

docname: draft-contreras-opsawg-scheduling-oam-tests-latest
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Operations and Management Area Working Group"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: "Operations and Management Area Working Group"
  type: "Working Group"
  mail: "opsawg@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/opsawg/"
  github: "vlopezalvarez/draft-contreras-opsawg-scheduling-oam-tests"
  latest: "https://vlopezalvarez.github.io/draft-contreras-opsawg-scheduling-oam-tests/draft-contreras-opsawg-scheduling-oam-tests.html"

author:
 -
    fullname: Luis M. Contreras
    organization: Telefonica
    email: "luismiguel.contrerasmurillo@telefonica.com"
 -
    fullname: Victor Lopez
    organization: Nokia
    email: "victor.lopez@nokia.com"

normative:

informative:


--- abstract

This document defines a YANG data model for network diagnosis on-demand using Operations, Administration, and Maintenance (OAM) tests. This document defines both 'oam-unitary-test' and 'oam-test-sequence' data models to enable on-demand activation of network diagnosis procedures.

The YANG data model defined in this document conforms to the Network Management Datastore Architecture (NMDA).


--- middle

# Introduction

twork. The OAM framework defines mechanisms for fault detection and isolation, such as continuity check, link trace, and loopback.
+ Performance management enables monitoring network performance and diagnosing performance issues (i.e., degradation). Some of the measurements such as frame delay measurement, frame delay variation measurement, and frame loss measurement.
- Security management defines mechanisms to protect OAM communications from unauthorized access and tampering.

{{!RFC6428}} defines several use cases for OAM tools, including:

* Continuity Check: This function verifies that a path exists between two points in a network and that the path is operational.
+ Loopback: This function allows a device to loop back a received packet back to the sender for diagnostic purposes.
+ Link Trace: This function allows a network operator to trace a path through a network from one device to another.
+ Performance Monitoring: This function allows a network operator to monitor the performance of a network and to identify and diagnose performance issues.
+ Security Management: This function allows a network operator to protect OAM communications from unauthorized access and tampering.
- Configuration Management: This function allows a network operator to manage the configuration of network devices.

On one hand {{!RFC8531}}, {{!RFC8532}} and, {{!RFC8533}} defined YANG models for OAM technologies. On the other hand, {{!RFC8531}} defines a YANG data model for connection-oriented OAM protocols. The main aim of this document is to define a generic YANG data model that can be used to configure, control and monitor connection-oriented OAM protocols such as MPLS-TP OAM, PBB-TE OAM, and G.7713.1 OAM. {{!RFC8532}} provides a generic YANG data model that can be used to configure, control and monitor connectionless OAM protocols such as BFD (Bidirectional Forwarding Detection), LBM (Loopback Messaging) and VCCV (Virtual Circuit Connectivity Verification). {{!RFC8533}} provides a YANG data model that can be used to retrieve information related to OAM protocols such as Bidirectional Forwarding Detection (BFD), Loopback Messaging (LBM) and Virtual Circuit Connectivity Verification (VCCV).

Previous RFCs defined the parameters required for each of the different tests that are used in network elements today. This document covers how to use OAM for network-wide use cases. Following, some illustrative examples are presented.

The YANG data model resulting from this document will conform to the Network Management Datastore Architecture (NMDA) {{!RFC8342}}.


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
