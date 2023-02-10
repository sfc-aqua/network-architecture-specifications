---
title: Quantum Network Architecture
abbrev: QNA
docname: draft-aqua-network-architecture
date: 2023-02-10
category: info

ipr: trust200902
area: Quantum
workgroup: AQUA
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: R. Van Meter
    name: Rodney Van Meter
    organization: Keio University
    street: 5322 Endo
    region: Fujisawa, Kanagawa
    code: 252-0882
    country: Japan
    email: rdv@sfc.wide.ad.jp

normative:
  RFC2119:
  I-D.setup:
    target: https://github.com/sfc-aqua/draft-quantum-connection-setup
    title: Connection Setup in a Quantum Network
    author:
      name: Rodney Van Meter
      ins: R. Van Meter
    author:
      name: Takaaki Matsuo
      ins: T. Matsuo
    date: 2021-03-18

informative:
  RFC9340:
  I-D.draft-irtf-qirg-principles:
  cocori-ms-thesis:
    target: TBD
    title: RuLa A Programming Language for RuleSet-based Quantum Repeaters
    author:
      name: Ryosuke Satoh
      ins: R. Satoh
    date: 2023-02-01
  rdv-qce-arch:
    target: https://arxiv.org/abs/2112.07092
    title: A Quantum Internet Architecture
    author:
      -
        name: Rodney Van Meter
        ins: R. Van Meter
        org: AQUA
      -
        name: Ryosuke Satoh
        ins: R. Satoh
        org: AQUA
      -
        name: Naphan Benchasattabuse
        ins: N. Benchasattabuse
        org: AQUA
      -
        name: Takaaki Matsuo
        ins: T. Matsuo
        org: AQUA
      -
        name: Michal Hajdušek
        ins: M. Hajdušek
        org: AQUA
      -
        name: Takahiko Satoh
        ins: T. Satoh
        org: AQUA
      -
        name: Shota Nagayama
        ins: S. Nagayama
        org: AQUA
      -
        name: Shigeya Suzuki
        ins: S. Suzuki
        org: AQUA
    date: 2021-12-14

--- abstract

A RuleSet-based quantum network architecture is described.

This architecture document does not cover internetworking, but is
internetworking-ready.

--- middle

Introduction        {#intro}
============


Glossary
========

* Channel
* Connection
* DistRuleSet
* Link
* Node
* Rule
* RuleSet
* State
* Tag

Rules and RuleSets
==================

Rules determine the actions to be executed on one or more quantum
states by quantum network nodes.  Rules are collected into RuleSets.
At a Node, each connection is governed by a RuleSet.

A DistRuleSet is the set of all RuleSets governing the operation of a
connection, at all nodes.

The detailed syntax and semantics of Rules and RuleSets are out of
scope for this document.  See {{cocori-ms-thesis}}.

Naming and identifiers
======================

Nodes
-----

Channels and links
--------------

Connection identifiers
----------------------

Rule identifiers
----------------

The scope for a RuleID is the DistRuleSet.


RuleSet identifiers
-------------------



Quantum state tags
------------------

Quantum network nodes build end-to-end shared quantum states to
delivered to applications, by using partial quantum states.  Nodes
exchange messages to facilitate this process.  Every quantum state
that is operated on based on the contents of a message received from
some partner, therefore, MUST have a TAG that can be referred to,
serving as the name for the state.

The TAG MUST be unique within a defined scope.  A state instantiated
locally as one or more qubits belongs to a RULE. The scope for the tag
is the RULE.

The contents of the TAG MAY be opaque to partner nodes, but a
receiving Node MUST be able to map the TAG to the physical qubit or
qubits currently holding the state.


Node type definitions
=====================

The first architecture concerns only four types of Nodes, described
below.  Eight more types of nodes have been defined in the research
literature and will be incorporated in future specifications
{{rdv-qce-arch}}.


MEAS
----

EPPS
----

BSA
----

OSW
----


Common node requirements
========================

Naming and addressing of nodes
------------------------------


Services and external communication methods
-------------------------------------------
(e.g., protocol and port number)


Neighbor discovery and type determination
-----------------------------------------

Routing
-------

Multiplexing
------------

Connection setup and teardown
-----------------------------

 (common portions)


White Rabbit absolute and relative clock synchronization
--------------------------------------------------------

Timing regimes
==============

The detailed operation of the interconnect at both the link and network levels involves hardware and software signals and events across many orders of magnitude difference in required latency. Some of the timing regimes are:

* interferometric stabilization: need for sub-wavelength stability is photonic qubit representation-dependent
* photon wave packet overlap: technology-dependent photon wave packet length, but roughly nanoseconds
* opening and closing of detector timing windows, detector recovery time: nanoseconds to microseconds
* measurement basis selection (if required in BSA): performance will constrain entanglement attempt rate
* optical switch control: switching of trains of wave packets, performance will constrain multiplexing and entanglement attempt rate
* pre-configured event-driven tasks such as timing-triggered or measurement-triggered execution of quantum circuits: microseconds
* urgent but not synchronous-critical tasks (e.g., execution of classical code that processes RuleSet messages and selects or creates new quantum circuits for execution): microseconds
* host-side application-level tasks (e.g. post-measurement operations): milliseconds
* background tasks (link tomography calculations, routing table updates): seconds to minutes

Some of these can only be achieved using high-quality hardware, while others are software tasks. Detailed analysis of these regimes will affect core software design in each network node type.

Quantum Router Software Architecture (QRSA)
===========================================

See Document 501 for an overview. To be adapted from existing QuISP documentation.
n.b.: no nodes in this demonstration network are actually routers; QRSA is simply the name of the software suite. Subsets of the QRSA functionality are implemented in dfferent ways for different node types.

MEAS
====

Connection setup Initiator
--------------------------

Connection setup Responder
--------------------------


RuleSets for measurement operations
--------------------------


Role in photonic path synchronization negotiation
--------------------------

EPPS
====

Connection setup relay
--------------------------

Role in photonic path synchronization negotiation
--------------------------

BSA
===

Connection setup relay
--------------------------

Role in photonic path synchronization negotiation
--------------------------

OSW
===

Connection setup relay
--------------------------

Role in photonic path synchronization negotiation
--------------------------

Signalling for circuit switching
--------------------------


Security
========

Node security
--------------------------

Protocol security
--------------------------

Network Monitoring
==================

Collection and reporting of performance statistics

--- back

