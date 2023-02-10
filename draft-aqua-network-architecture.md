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

--- abstract

A RuleSet-based quantum network architecture is described.


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

Quantum network nodes build shared quantum states.
Every quantum state that is operated on based on the contents of a
message received from some partner, therefore, MUST have a TAG that
can be referred to, serving as the name for the state.

The TAG MUST be unique within a defined scope.  As a state,
instantiated locally as one or more qubits, belongs, to a RULE, the
scope for the tag is the RULE.


Node type definitions
=====================

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

