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
    city: Fujisawa
    country: Japan
    email: rdv@sfc.wide.ad.jp

normative:
  RFC2119:

informative:
  RFC9340:


--- abstract


Network performance and demonstration requirements: to be adapted from documents produced by the entire group and by PM Nagayama.

--- middle

Introduction        {#intro}
============


Glossary
========

Naming and identifiers
======================

Nodes
-----

Channels/links
--------------

Connections
-----------

Quantum states
--------------

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

