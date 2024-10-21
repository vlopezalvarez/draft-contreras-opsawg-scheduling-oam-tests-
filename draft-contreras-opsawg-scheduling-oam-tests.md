---
title: "A YANG Data Model for Network Diagnosis using Scheduled Sequences of OAM Tests"
abbrev: "Scheduling OAM YANG"
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
 - OAM
 - Scheduling
 - Test sequences
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

   ITU-T-Y1731:
              title: "OAM Functions and Mechanisms for Ethernet-based
                 Networks"
              date: 2023-06-13
              target: https://www.itu.int/rec/T-REC-Y.1731/en

--- abstract

This document defines a YANG data model for network diagnosis on-demand relying upon Operations, Administration, and Maintenance (OAM) tests. This document defines both 'oam-unitary-test' and 'oam-test-sequence' YANG modules to manage the lifecycle of network diagnosis procedures.

--- middle

# Introduction

Operations, Administration, and Maintenance (OAM) tasks are fundamental functions of the network management (see, e.g., {{?RFC7276}}). Given the emergence of data models and their utilization in Service Provider's network management and the need to automate the overall service management lifecycle {{?RFC8969}}, the management of OAM operations becomes also key. Relevant data models are still missing to cover specific needs.

Specifically, OAM functions provide the means to identify and isolate faults, measure and report of performance (see section 4.2, {{?RFC6632}}. For example, {{!RFC5860}} defines the three main areas involved in OAM:
* Fault management, which allows network operators to quickly identify and isolate faults in the network. Examples of these mechanisms for fault detection and isolation are: continuity check, link trace, and loopback.
+ Performance management enables monitoring network performance and diagnosing performance issues (i.e., degradation). Some of the measurements such as frame delay measurement, frame delay variation measurement, and frame loss measurement.
- Security management defines mechanisms to protect OAM communications from unauthorized access and tampering.

{{?RFC7276}} presents OAM tools for detecting and isolating failures in networks and for performance monitoring, some examples are:
* Continuity Check: This function verifies that a path exists between two points in a network and that the path is operational.
+ Loopback: This function allows a device to loop back a received packet back to the sender for diagnostic purposes. There are multiple technologies for this function, like IP Ping, VCCV Ping, LSP Ping or Ethernet Loopback.
+ Link Trace: This function allows a network operator to trace a path through a network from one device to another. Some technologies following this approach are Y.1731 Linktrace {{ITU-T-Y1731}} or IP traceroute.
- Performance Monitoring: This function allows a network operator to monitor the performance of a network and to identify and diagnose performance issues. Protocols like TWAMP, or Y.1731 DMM/SLM {{ITU-T-Y1731}} can obtain performance measurements.

More recently, Incident Management {{?I-D.ietf-nmop-network-incident-yang}} focuses on
   of incident diagnosis, which can be favored by dynamic invocation of OAM tests.

{{!RFC8531}}, {{!RFC8532}}, {{!RFC8533}} and {{!RFC8913}}  defined YANG models for OAM technologies:
+ {{!RFC8531}} defines a YANG data model for connection-oriented OAM protocols. The main aim of this document is to define a generic YANG data model that can be used to configure, control and monitor connection-oriented OAM protocols such as MPLS-TP OAM, PBB-TE OAM, and G.7713.1 OAM.
+ {{!RFC8532}} provides a generic YANG data model that can be used to configure, control and monitor connectionless OAM protocols such as BFD (Bidirectional Forwarding Detection), LBM (Loopback Messaging) and VCCV (Virtual Circuit Connectivity Verification).
+ {{!RFC8533}} provides a YANG data model that can be used to retrieve information related to OAM protocols such as Bidirectional Forwarding Detection (BFD), Loopback Messaging (LBM) and Virtual Circuit Connectivity Verification (VCCV).
+ {{!RFC8913}} specifies a YANG data model for client and server implementations of the Two-Way Active Measurement Protocol (TWAMP).
These RFCs defined the parameters required for each of the different tests that are used in network elements today. This document covers how to use OAM for network-wide use cases. Following, some illustrative examples are presented.

The YANG data model resulting from this document will conform to the Network Management Datastore Architecture (NMDA) {{!RFC8342}}.

## Terminology and Notations

This document assumes that the reader is familiar with the contents of {{!RFC7950}}, {{!RFC8345}}, {{!RFC8346}} and {{!RFC8795}}.

Following terms are used for the representation of this data model.
* OAM unitary test: A set of parameters that define a type of OAM test to be invoked. As an example, it includes the type test, configuration parameters, and target results.
* OAM test sequence: A set of OAM unitary tests that are run based on a set of time constraints, number of repetitions, order, and reporting outputs.

Tree diagrams used in this document follow the notation defined in {{!RFC8340}}.

## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in  {{!RFC2119}}, {{!RFC8174}} when, and only when, they appear in all capitals, as shown here.

## Prefix in Data Node Names

  In this document, names of data nodes and other data model objects will be prefixed using the standard prefix associated with the corresponding YANG imported modules, as shown in the following table.

| Prefix | Yang Module            | Reference    |
| ------ | ---------------------- | ------------ |
| oamut  | ietf-oam-unitary-tests | RFCXXXX      |
| oamts  | ietf-oam-test-sequence | RFCXXXX      |
| yang   | ietf-yang-types        | {{!RFC6991}} |
{: #tab-prefixes title="Prefixes and Corresponding YANG Modules"}

> RFC Editor Note:
> Please replace XXXX with the RFC number assigned to this document if the document becomes a RFC. Please remove this note in that case.

# Network-wide OAM Use Cases

## Troubleshooting

After the detection of a problem {{?I-D.ietf-nmop-terminology}} in the network, OAM tests are performed to find the root cause for the detected problem. However, a detected problem can be caused by a variety of factors, such as a misconfiguration, hardware failure, or a software bug. OAM tests can help to find the root cause by testing specific components of the network and looking for anomalies or issues. Also, the reliability and efficiency of the tests depend on the nature of the test itself.

There are a variety of OAM tests that can be executed as a function of the target scenario. For example, if the issue is related to a Layer 2 capability, specific tests can be designed and run to check the status of the path via Ethernet Linktrace and later run an Ethernet Loopback to a concrete network element. These tests can be coupled with others to test if any filtering is in place by varying, e.g., some Layer 2 fields or checking the configuration of relevant nodes.  If these tests are correct, the operator may want to check the availability of the service (or its delivered performance).

Even though the troubleshooting process may be different depending on the problem detected, there are certain common procedures or logics that can be executed in order to narrow down the cause of the problem and thus help isolating candidate root cause.

## Birth Certificate

The aim of a birth certificate process is to validate that all relevant parameters are set appropriately in accordance with the target network service. The birth certificate process is done once the configuration of the network elements is completed, and they are ready for service.

If the birth certificate is successful, it means that the network service is functioning correctly (that is, measured service is matching the expected service) and meets the requirements defined by the operator. The process requires running a set of OAM tasks (e.g., tests) to verify that the service is performing as expected.

The set of OAM tests conducted as part of a birth certificate process depends on the network service that is tested.  For example, if the service is a Virtual Private Network (VPN). Two-Way Active Measurement Protocol (TWAMP) Light {{!RFC5357}} will be used, while if the service is an E-LINE, ITU-T Y.1731 Ethernet CFM tests {{ITU-T-Y1731}} will be executed.

Typically, once the birth certificate process has been completed and the OAM tests have been executed, the test results are stored as part of the documentation process performed by the operator. Many of these tasks take place during pre-deployment phases.

## Proactive Supervision

Some network services require to fulfill strict Service Level Agreements (SLAs).  An SLA defines the performance parameters that the service must fulfill in order to meet the requirements of the customer or end user (e.g., IP Connectivity Provisioning Profile (CPP) {{?RFC7297}} and Network Slice Service {{?RFC9543}}).

As part of service fulfillment and assurance (e.g., Section 2.3.3 of {{?RFC4176}}), proactive verification is undertaken to assess whether SLAs are met and implement appropriate adjustment measures when service distortion is observed. Proactive supervision requires running tests both end-to-end, but also on service components to identify early symptoms and resolve issues before they impact the customer or end user, or to prevent or minimize the impact of the end user. Mitigation action may be enforced to soften the impact of networks incidents and soften/nullify the impact on services that are delivered via that network.

Proactive testing might be done via OAM tests. These tests can be run periodically at regular intervals depending on the specific SLA requirements and the network operator procedures. These procedures may require documenting the test results for future auditing processes with the customers (eventually, negotiated and agreed with a customer as part of service assurance).

## Performance-based Path Routing

Path Computation Elements (PCEs) are used to compute end-to-end paths in a network {{?RFC4655}}. PCEs are used for Traffic Engineering (TE) purposes (e.g., optimize network performance, reduce congestion, and improve the overall user experience).

There are different algorithms to calculate a path in the network for some of them the PCE requires traffic engineering information. TE information includes data such as link metrics, bandwidth availability, and routing constraints. By using this information, the PCE can compute the optimal path for a particular service, taking into account its constraints and requirements. OAM techniques allow obtaining link metrics like delay and loss which can be used in the PCE algorithms.

# Modelling the Scheduling of OAM Tests

This document specifies two models: OAM unitary test and OAM test sequence models.

## OAM Unitary Test

The OAM unitary test model encompasses parameters that define a specific type of OAM test to be performed. The YANG model includes a container named "oam-unitary-tests" that serves as a container for activating OAM unitary tests for network diagnosis procedures. Inside the container, there is a list called "oam-unitary-test" representing a list of specific OAM unitary tests. The list key is defined as "name", which provides a unique name for each test. Each OAM test in the list references a test type with its concrete parameters. The test types are out of the scope of this document. Moreover, each OAM unitary test has two temporal parameters: "period-of-time" and "recurrence". Both are imported from the "ietf-schedule" module from {{!I-D.ietf-netmod-schedule-yang}}. "period-of-time" identifies the period values that contain a precise period of time, while "recurrence" identifies the properties that contain a recurrence rule specification. "unitary-test-status" enumerates the state of the OAM unitary test.

{{oam-uni-test-tree-st}} shows the structure of OAM unitary test module:

~~~~
module: ietf-oam-unitary-test
  +--rw oam-unitary-tests
     +--rw oam-unitary-test* [name]
        +--rw name                      string
        +--rw ne-config* [ne-id]
           +--:(test-type)
        +--rw period-description?       string
        +--rw period-start              yang:date-and-time
        +--rw time-zone-identifier?     sys:timezone-name
        +--rw (period-type)?
        |  +--:(explicit)
        |  |  +--rw period-end?         yang:date-and-time
        |  +--:(duration)
        |     +--rw duration?           duration
        +--rw recurrence-description?   string
        +--rw frequency                 identityref
        +--rw interval?                 uint32
        +--rw unitary-test-status?      enumeration
~~~~
{: #oam-uni-test-tree-st title="Tree Structure of OAM Unitary Test" artwork-align="center"}

(Note: alignment with {{!I-D.ietf-netmod-schedule-yang}} will be done with the progress of that document).
The 'unitary-test-status' state machine is shown in {{st-unitary-test-status}}. The state machine includes the following states:
* "planned": The initial state where the test is planned.
* "configured": The state where the test is being configured.
* "ready": The state where the test is ready to be executed.
* "on-going": The state where the test is currently running.
* "stop": The state where the test is manually stopped.
* "error": The state where an error occurs during the test.
* "finished": The final state where the test is completed.

~~~~

   +---------+      +----------+      +---------+
+->| planned |----->|configured|----->|  ready  |
|  +---------+      +----------+      +---------+
|                        |                |
|                        |                V
|            +-------+   |          +----------+
|      +-----| error |<--+----------| on-going |
|      |     +-------+              +----------+
|      |                                  |
|      V                                  |
|  +---------+      +--------+            |
+--| finished|<-----|  stop  |<-----------+
   +---------+      +--------+            |
       A                                  |
       |                                  |
       +----------------------------------+

~~~~
{: #st-unitary-test-status title="OAM Unitary Test State Machine" artwork-align="center"}

## OAM Test Sequence

The OAM test sequence model consists of a collection of OAM unitary tests that are executed based on specified time constraints, repetitions, ordering, and reporting outputs. These sequences provide a structured approach to running multiple OAM tests in a coordinated manner.

Each OAM test sequence references an OAM unitary test type with its concrete parameters. Each OAM test sequence has two temporal parameters: "period-of-time" and "recurrence". Both are imported from the "ietf-schedule" module from {{!I-D.ietf-netmod-schedule-yang}}. "period-of-time" identifies the period values that contain a precise period of time, while "recurrence" identifies theproperties that contain a recurrence rule specification. "unitary-test-status" enumerates the state of the OAM test. Finally, "test-sequence-status" shows the state of the OAM test sequence.

{{oam-test-sequence-tree-st}} shows the structure of OAM test sequence module:

~~~~

module: ietf-oam-test-sequence
  +--rw oam-test-sequence
     +--rw test-sequence* [name]
        +--rw name                      string
        +--rw test-ref* [name]
        |  +--rw name             string
        |  +--rw (test-type)
        |  +--rw numexecutions?   uint32
        +--rw period-description?       string
        +--rw period-start              yang:date-and-time
        +--rw time-zone-identifier?     sys:timezone-name
        +--rw (period-type)?
        |  +--:(explicit)
        |  |  +--rw period-end?         yang:date-and-time
        |  +--:(duration)
        |     +--rw duration?           duration
        +--rw recurrence-first
        |  +--rw utc-start-time?   yang:date-and-time
        |  +--rw duration?         uint32
        +--rw (recurrence-bound)?
        |  +--:(until)
        |  |  +--rw utc-until?          yang:date-and-time
        |  +--:(count)
        |     +--rw count?              uint32
        +--rw recurrence-description?   string
        +--rw frequency                 identityref
        +--rw interval?                 uint32
        +--rw test-sequence-status?     enumeration

~~~~
{: #oam-test-sequence-tree-st title="OAM test sequence" artwork-align="center"}


The 'test-sequence-status' state machine is shown in {{st-test-sequence-status}}. The state machine includes the following states:
* "planned": The initial state where the test is planned.
* "configured": The state where the test is being configured.
* "ready": The state where the test is ready to be executed.
* "on-going": The state where the test is currently running.
* "stop": The state where the test is manually stopped.
* "success": The state when all unitary tests were successful.
* "failure": The state when one or more tests in the sequence got an error.
* "error": The state where an error occurs during the test.

~~~~

    +---------+      +----------+      +---------+
 +->| planned |----->|configured|----->|  ready  |
 |  +---------+      +----------+      +---------+
 |    A   A                 |              |
 |    |   |                 |              V
 |    |   |     +-------+   |          +----------+
 |    |   ------| error |<--+----------| on-going |
 |    |         +-------+              +----------+
 |    |                                    |
 |    |                                    |
 | +---------+      +--------+             |
 | | failure |<-----|  stop  |<------------+
 | +---------+      +--------+             |
 |                                         |
 | +---------+                             |
 +-| success |<----------------------------+
   +---------+

~~~~
{: #st-test-sequence-status title="OAM test sequence state machine" artwork-align="center"}


# YANG Data Models for Scheduling OAM Tests

## YANG Model for Scheduling OAM Unitary Test

~~~~~~~~~~
<CODE BEGINS> file ietf-oam-unitary-test@2024-07-05.yang
{::include ./Yang/ietf-oam-unitary-test.yang}

<CODE ENDS>
~~~~~~~~~~

## YANG Model for OAM Test Sequence

~~~~~~~~~~
<CODE BEGINS> file ietf-oam-test-sequence@2024-07-05.yang

{::include ./Yang/ietf-oam-test-sequence.yang}

<CODE ENDS>
~~~~~~~~~~

# Using Device Mode Within OAM Scheduling Models

This section discusses the issues related to reusing device models already defined in IETF within the context of scheduling OAM tests. There are two main approaches to enable OAM scheduling models:
  * Importing YANG model into the OAM scheduling models. This approach will copy the device model into the OAM unitary test model to enable the configuration and utilization of the desired OAM test. This approach requires to recreate new YANG models for each new test type or variation of the device models.
  * Schema-mount allows mounting a data model at a specified location of another (parent) schema. The main difference with importing the YANG modules is that they don't have to be prepared for mounting; any existing modules such as "ietf-twamp" can be mounted without any modifications.

As an exmaple, we will use {{!RFC8913}}, which defines a YANG data model for TWAMP, to illustrate how device models could be used.


# Security Considerations

The YANG module targeted in this document defines a schema for data that is designed to be accessed via network management protocols such as NETCONF {{!RFC6241}} or RESTCONF {{!RFC8040}}.  The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{!RFC6242}}. The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{!RFC5246}}.

The NETCONF access control model {{!RFC6536}} provides the means to restrict access for particular NETCONF or RESTCONF users to a preconfigured subset of all available NETCONF or RESTCONF protocol operations and content.

There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., config true, which is the default).  These data nodes may be considered sensitive or vulnerable in some network environments.  Write operations (e.g., edit-config) to these data nodes without proper protection can have a negative effect on network operations.

In which refers to the scheduling of the tests, security considerations in {{!I-D.ietf-netmod-schedule-yang}} are also applicable here.

# IANA Considerations

TBC

# Implementation Status

This section will be used to track the status of the implementations of the model. It is aimed at being removed if the document becomes RFC.

--- back

# Examples {#examples}

This section includes a non-exhaustive list of examples to illustrate the use of the models defined in this document.

## Create a TWAMP OAM test {#ex-create-twp-oam}

{{!RFC8913}} defines a YANG model for TWAMP. There is an example for TWAMP. The following example contains the information for the four configurations (Control-Client, Server, Session-Sender and Session-Reflector).

An example of a request message body to create a TWAMP OAM test is shown in {{create-twp-oam}}. Session-Sender and Session-Reflector as expanded for illustrative purposes. The TWAMP Test scheduled in this configuration is a one-hour performance monitoring test that runs daily at 9 AM UTC. This test session is configured to start on October 17, 2023, at 09:00 UTC and recur at the same time every day. The duration of each test run is one hour, as specified by the ISO 8601 format "PT1H", with the test status marked as "scheduled". The test provides insight into network performance by monitoring the selected parameters, allowing for the detection of any potential degradations in service quality over time.

~~~~ json
{::include-fold ./json-examples/create-twp-oam.json}
~~~~
{: #create-twp-oam title="Example of a Message Body to Create a TWAMP OAM test"}

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
