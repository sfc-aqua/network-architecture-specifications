---
title: Quantum Network Architecture
abbrev: QNA
docname: draft-aqua-network-architecture-00
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
  marchese-tannenbaum-van-steen-summary:
    target: https://csis.pace.edu/~marchese/CS865/Lectures/Chap5/Chapter5a.htm
    title: CS865 – Distributed Software Development, Lecture 5, Tannenbaum and Van Steen – Chapter 5
    author:
      name: Francis T. Marchese
      ins: F. T. Marchese

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

The common use case for this network is the creation of Bell pairs,
bipartite states with one qubit at one EndQNode and one qubit and
another EndQNode.  The architecture, but not necessarily the initial
instances of all underlying protocols, supports multi-partite states
shared among more than two nodes.

--- middle

Introduction        {#intro}
============

A QNode on a quantum network has at least one interface that accepts or
emits quantum states to be shared with other QNodes.  Typically, but
not always, that interface will be optical.  This document assumes the
use of photons, described as quantum wave packets, for quantum
communication, and includes constraints on them.

Each QNode is controlled by one classical controller known as a CNode.
A single CNode MAY control more than one QNode.

Many operations require that CNodes within a quantum network
communicate with each other.  That communication MAY be via TCP/IP.


Glossary
========

* Channel
* Connection
* DistRuleSet
* Link
* QNode
* Rule
* RuleSet
* Stage
* State
* Tag
* WavePacket
* WavePacketTrain

Rules and RuleSets
==================

Rules determine the actions to be executed on one or more quantum
states by quantum network nodes.  Each Rule has a Condition Clause and
an Action Clause. Rules are collected into RuleSets.  At a QNode, each
connection is governed by a RuleSet.

A DistRuleSet is the set of all RuleSets governing the operation of a
connection, at all nodes.

The detailed syntax and semantics of Rules and RuleSets are out of
scope for this document.  See {{cocori-ms-thesis}}.

Naming and identifiers
======================

QNodes
-----

A QNode MUST have a identifier
{{marchese-tannenbaum-van-steen-summary}}.  The identifier MUST be
unique within the corresponding scope.  A QNode MUST have only one
identifier within a given scope.  A QNode MAY participate in more than
one scope.  This identifier is the QNodeID.

It is NOT RECOMMENDED that IP addresses be used unmodified as QNodeIDs.
A single host on an IP network can have multiple IP addresses on which
it can receive messages about a single attached quantum QNode, so
multiple identifiers would correspond to the same QNode.  A single host
on an IP network can serve as the classical control for multiple
quantum QNodes, so an IP address as QNodeID also would not uniquely
specify the correct QNode.

An IP address MAY form part of the QNodeID.
  
Channels and links
--------------

Connection identifiers
----------------------

Rule identifiers
----------------

A RuleID identifies a specific Rule within a RuleSet.

The scope for a RuleID is the DistRuleSet.


RuleSet identifiers
-------------------

A RuleSetID identifies a specific RuleSet.

The scope of the RuleSetID is...

Quantum state tags
------------------

Quantum network nodes build end-to-end shared quantum states to
delivered to applications, by using partial quantum states.  QNodes
exchange messages to facilitate this process.  Every quantum state
that is operated on based on the contents of a message received from
some partner, therefore, MUST have a TAG that can be referred to,
serving as the name for the state.

The TAG MUST be unique within a defined scope.  A state instantiated
locally as one or more qubits belongs to a RULE. The scope for the tag
is the RULE.

The contents of the TAG MAY be opaque to partner nodes, but a
receiving QNode MUST be able to map the TAG to the physical qubit or
qubits currently holding the state.

A TAG MAY be a simple sequence number, provided that sequence numbers
are unique and are not reused over the lifetime of the Connection.

Rewriting of state tags.

AllSwap.



Classes of QNodes
=================

* QEndNode
* QRepeaterNode
* QSupportNode

QEndNode type definitions
=====================

The first architecture concerns only four types of QNodes, described
below.  Eight more types of nodes have been defined in the research
literature and will be incorporated in future specifications
{{rdv-qce-arch}}.

MEAS
----

QRepeaterNode type definitions
=============================

No QRepeaterNode types are defined in this document.


QSupportNode type definitions
=============================

EPPS
----

BSA
----

OSW
----


Common node requirements {#node-req}
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

Bipartite connection setup and teardown
-----------------------------

 (common portions)


White Rabbit absolute and relative clock synchronization
--------------------------------------------------------

Quantum Router Software Architecture (QRSA)
===========================================

See Document 501 for an overview. To be adapted from existing QuISP documentation.

This document specifies node requirements in terms of externally
visible functionality, rather than mandating an implementation.  The
Quantum Router Software Architecture (QRSA) is the name of a reference
software implementation that conforms to this behavioral
specification, particularly the common node requirements described in
{{node-req}}.

Subsets of the QRSA functionality are implemented in dfferent ways for
different node types.  Not all node types are required to implement
all functions.

Timing regimes {#timing}
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


Classical communication
=======================

Many operations require that CNodes within a quantum network
communicate with each other.  The different levels of timing
constraint described in {{timing}} correspond to classical
communication requirements.

Some communication MAY be via TCP/IP.

Some communication requires control messages with timing that is
indistinguishable from one or more corresponding quantum signals
carried in a quantum channel.  The more straightforward implementation
will be to carry such signals in the same physical channel,
multiplexed by time, wavelength, or similar approaches.

Classical heralding of WavePacket timing.

Timeouts and race conditions
============================

Security
========

QNode security
--------------------------

Protocol security
--------------------------

Network Monitoring
==================

Collection and reporting of performance statistics

--- back

