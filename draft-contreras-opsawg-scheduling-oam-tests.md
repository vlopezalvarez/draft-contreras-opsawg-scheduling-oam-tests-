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

Operations, Administration, and Maintenance (OAM) tasks are fundamental functionalities of the network management. Given the emerging of data models and their utilization in Service Provider's management, the management of OAM tests represent also an area of interest for operators, which requires to be defined as a data model. OAM functionalities provide the means to identify and isolate faults, measure and report of performance. {{!RFC5860}} defines the three main areas involved in OAM:

* Fault management, which allows network operators to quickly identify and isolate faults in the network. The OAM framework defines mechanisms for fault detection and isolation, such as continuity check, link trace, and loopback.
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

## Terminology and Notations 

This document assumes that the reader is familiar with the contents of {{!RFC7950}}, {{!RFC8345}}, {{!RFC8346}} and {{!RFC8795}}.

Following terms are used for the representation of this data model.

* OAM unitary test: it is a set of parameters that define a type of OAM test to be invoked. As an example, it includes the type test, configuration parameters, and target results.

* OAM test sequence: it is a set of OAM unitary tests that are run based on a set of time constraints, number of repetitions, order, and reporting outputs.

## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in  {{!RFC2119}}, {{!RFC8174}} when, and only when, they appear in all capitals, as shown here.

## Prefix in Data Node Names

  In this document, names of data nodes and other data model objects will be prefixed using the standard prefix associated with the corresponding YANG imported modules, as shown in the following table.

| Prefix | Yang Module            | Reference    |
| ------ | ---------------------- | ------------ |
| oamut  | ietf-oam-unitary-tests | RFCXXX       |
| oamts  | ietf-oam-test-sequence | RFCXXX       |
| yang   | ietf-yang-types        | {{!RFC6991}} |
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document if the document becomes a RFC. Please remove this note in that case.

# Network wide OAM use cases

## Troubleshooting

After the detection of a problem in the network, OAM tests are performed to find the root cause for the detected issue. However, a problem detected can be caused by a variety of factors, such as a misconfiguration, hardware failure, or a software bug. OAM tests can help to find the root cause by testing specific components of the network and looking for anomalies or issues.

There are a variety of different OAM tests that can be executed depending on the nature of the scenario. For example, if the issue is related to a L2 capability, tests can be run to check the status of the path via Ethernet Linktrace and later run an Ethernet Loopback to a concrete network element. If these tests are correct, the operator may want to check the availability of the service or its performance.

Even though the troubleshooting process may be different depending on the problem detected, there are certain common procedures or logic that can be executed in order to narrow down the cause of the problem.

## Birth certificate

The aim of a birth certificate process is to validate that all relevant parameters are correct for a specific network service. The birth certificate process is done once the configuration of the network elements is completed and they are ready for service. 

If the birth certificate is successful, it means that the network service is functioning correctly and meets the requirements defined by the operator. The process requires running a set of OAM tests to verify that the service is performing as expected.

The set of OAM tests done as part of a birth certificate process depends on the network service that is tested. For example, if the service is a virtual private network (VPN) Two-Way Active Measurement Protocol (TWAMP) Light will be used, while if the service is an E-LINE, Ethernet CFM tests will be executed.

Once the birth certificate process has been completed and the OAM tests have been run, the test results are stored as part of the documentation process performed by the operator.

## Proactive supervision

There are communication services that require to fulfill Service Level Agreements (SLAs). SLAs define performance parameters that the service must fulfill in order to meet the requirements of the customer or end user.

Proactive testing ensures the SLAs are met. Proactive supervision requires running tests on service components to identify and resolve issues before they impact the customer or end user, or to minimize the impact of the end user.

Proactive testing can be done via OAM tests. These tests can be run periodically at regular intervals depending on the specific SLA requirements and the network operator procedures. These procedures may require documenting the test results for future auditing processes with the customers.

## Performance-based Path Routing 

Path Computation Elements (PCEs) allow computing end-to-end paths in a network. PCEs are used to facilitate traffic engineering and can be used to optimize network performance, reduce congestion, and improve the overall user experience.

There are different algorithms to calculate a path in the network for some of them the PCE requires traffic engineering information. TE information includes data such as link metrics, bandwidth availability, and routing constraints. By using this information, the PCE can compute the optimal path for a particular service, taking into account its constraints and requirements. OAM techniques allow obtaining link metrics like delay and loss which can be used in the PCE algorithms.

# YANG Data Model for scheduling OAM Tests


## YANG Model Overview

TBC

## Tree Diagram for scheduling OAM Tests

TBC

## YANG Model for scheduling OAM Tests

TBC


# Security Considerations

The YANG module targeted in this document defines a schema for data that is designed to be accessed via network management protocols such as NETCONF {{!RFC6241}} or RESTCONF {{!RFC8040}}.  The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{!RFC6242}}. The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{!RFC5246}}.

The NETCONF access control model {{!RFC6536}} provides the means to restrict access for particular NETCONF or RESTCONF users to a preconfigured subset of all available NETCONF or RESTCONF protocol operations and content.

There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., config true, which is the default).  These data nodes may be considered sensitive or vulnerable   in some network environments.  Write operations (e.g., edit-config) to these data nodes without proper protection can have a negative effect on network operations.


# IANA Considerations

TBC

# Implementation Status

This section will be used to track the status of the implementations of the model. It is aimed at being removed if the document becomes RFC.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
