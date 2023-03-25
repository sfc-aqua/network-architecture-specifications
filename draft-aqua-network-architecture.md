---
title: Quantum Network Architecture
abbrev: QNA
docname: draft-aqua-network-architecture-00
date: 2023-03-15
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
  RFC5151:
  RFC6001:
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
  wehner-science: DOI.10.1126/science.aam9288
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
  altepeter-tomography:
    target: TBD
    title: Photonic state tomography
    author:
      -
        name: Joseph B Altepeter
        ins: J. B. Altepeter
      -
        name: Evan R Jeffrey
        ins: E. R. Jeffrey
      -
        name: Paul G Kwiat
        ins: P. G. Kwiat
    journal: Advances in Atomic, Molecular, and Optical Physics
    volume: 52
    pages: 105--159
    date: 2005
  eisert-certification:
    title: Quantum certification and benchmarking
    author:
      name: Eisert, Jens and Hangleiter, Dominik and Walk, Nathan and Roth, Ingo and Markham, Damian and Parekh, Rhea and Chabaud, Ulysse and Kashefi, Elham
      ins: J. Eisert
    journal: Nature Reviews Physics
    volume: 2
    number: 7
    pages: 382--390
    date: 2020
  oka-qcit:
    title: A Classical Network Protocol to Support Distributed Quantum State Tomography
    author:
      -
        name: Takafumi Oka
        ins: T. Oka
      -
        name: Takahiko Satoh
        ins: T. Satoh
      -
        name: Rodney Van Meter
        ins: R. Van Meter
    booktitle: Proc. Quantum Communications and Information Technology
    date: 2016-12-01

--- abstract

A first generation (1G) RuleSet-based quantum network architecture is
described.  1G networks use an acknowledged link layer to handle loss
of photons, entanglement swapping to extend entanglement over multiple
hops, and purification for handling operational errors.

This network architecture provides entanglement as a service.
Applications may treat a network based on this architecture as
_prepare and measure_ or _entanglement generation_ stages of quantum
network evolution before nodes with memory are deployed, or a _quantum
memory_ network or above as memory nodes are deployed.

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

Words such as MUST and MAY are used in this document as specified in
RFC2119 {{RFC2119}}.

The goal of a quantum network is to enable distributed quantum
functions by distributing entangled quantum states
{{I-D.draft-irtf-qirg-principles}}.  These states may be measured
immediately upon arrival at the end nodes, or may be stored for
further manipulation, processing and later measurement
{{wehner-science}}.

A *QNode* on a quantum network has at least one interface that accepts or
emits quantum states to be shared with other QNodes.  Typically, but
not always, that interface will be optical.  This document assumes the
use of photons, described as quantum wave packets, for quantum
communication, and includes constraints on them.

Each QNode is controlled by one classical controller known as a CNode.
A single CNode MAY control more than one QNode.

Many operations require that CNodes within a quantum network
communicate with each other.  That communication MAY be via TCP/IP or
via low-level digital or analog signals, depending on context.

A quantum state known as a BasePair is assigned to a Rule within a
RuleSet immediately upon creation.  A state always belongs to exactly
one Rule.


2024 Network Goal
========


<figure>
<name>NMS View of Device State</name>
<artset>
<artwork type="svg" src="moonshot-local-network-switched-MIM.drawio.svg"/>
<artwork type="ascii-art">
Artwork only available as SVG (PDF and HTML)
</artwork>
</artset>
</figure>

This network diagram doesn't really belong in a document about
architecture, but this network is the target for the Moonshot in 2024.

The network will consist of:

* three End Nodes, each of which includes an entangled photon pair
  source (EPPS) and measurement device capable of measuring in
  multiple bases (MEAS)
* one Switched Bell State Analyzer, consisting of an optical switch
  (OSW) and one or more Bell State Analyzers (BSAs)

Thus, priority for development of detailed specifications focuses on
MEAS, EPPS, OSW and BSA functionality.

Glossary
========

* *BasePair*: an entangled two-qubit state established across a physical
  link.
* *Channel*: the physical channel (optical fiber or free space) that
  carries single-photon states.
* *Connection*: the software and soft state at two or more nodes that
  collectively works toward satisfying an application request for
  shared, distributed entangled states.  A Connection has an identifier.
* *DistRuleSet*: the set of RuleSets that collectively define a
  Connection.  The set of all RuleSets that share the same ConnectionID.
* *Initiator*: The node that creates a request for a connection.
* *Link*: A quantum channel plus its end points, capable of creating
  two-party entangled quantum states. A Link has characteristics such
  as the rate at which it can attempt to make entanglement, the
  success probability and fidelity.  n.b.: This is *not* a single
  Bell pair. (n.b.2: But, a Link may be virtualized when recursive
  networking is introduced.)
* QNode
* Responder
* Rule: a Condition Clause-Action Clause pair
* RuleSet
* Stage
* State
* Tag
* WavePacket
* WavePacketTrain

Supported applications
======================

Using the Wehner-Elkouss-Hanson taxonomy, prepare and measure networks
provide functionality sufficient for quantum key distribution and
secure identification {{wehner-science}}.  Entanglement generation
networks provide functionality sufficient for device-independent
protocols.  Quantum memory networks provide functionality sufficient
for a limited form of blind quantum computation and simple leader
election protocols.  Once quantum error correction is included,
networks become sufficient for all forms of distributed quantum
computing.

Connections and connection management
==================

Distributed quantum services and applications depend upon the ability
of a quantum network to create distributed entanglement.  In 1G
networks, this distribution process requires the cooperation of all of
the nodes along a path.  Therefore, quantum communication sessions are
stateful, and are described as *Connections*.

A Connection has the following characteristics:

* It executes on a set of QNodes that is a subset of the nodes in a
  quantum network.  Those nodes collectively form the *QPath*.  (A
  QPath may be a single path or a multipath.)
* It has an identifier.
* It utilizes quantum resources.
* It is governed by *Rules* collected into *RuleSets*.  The set of
  RuleSets for a Connection is called a DistRuleSet.
* It is created at the request of an *Initiator*.
* The RuleSets are created and distributed by a *Responder* to all of
  the nodes in the Connection.

Phases of connection operation
-----

A Connection passes through three phases during its lifetime:

* Connection setup: from the time the Initiator sends the request,
  through the Responder's distribution of RuleSets, to the start of
  real-time operations.  Connection setup messages and sub-phases are
  described in a separate specification {{I-D.setup}}.
* Execution: Real-time creation of distributed quantum states,
  including execution of Rules and exchange of classical messages.
* Shutdown: After completion of the quantum state creation and
  operations, the Connection MUST be shut down and resources
  recovered.

Classical timeout and recovery
-----

When a QNode loses contact with one or more nodes with whom it must
communicate during the Execution phase, after a timeout, it SHOULD
tear down the Connection and recover any resources (e.g., memory or
link utilization time slots) and return them to its free pool of
resources for reassignment.  AAA behavior on classical timeout is
undefined.

Rules and RuleSets
==================

*Rules* determine the actions to be executed on one or more quantum
states by quantum network nodes, with the goal of creating end-to-end
entanglement for a particular connection.  Each Rule has a Condition
Clause and an Action Clause.

Rules are collected into *RuleSets*.  At a QNode, each
connection is governed by a RuleSet.  RuleSets are created by a
Responder.

A *DistRuleSet* is the set of all RuleSets governing the operation of a
connection, at all nodes.

RuleSets do not specify routing, multiplexing or security.

The detailed syntax and semantics of Rules and RuleSets are out of
scope for this document.  See {{cocori-ms-thesis}}.

suz: add example pseudocode here

Naming and identifiers
======================

suz: need to clarify what applies hop-by-hop and what is E2E.

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


The first architecture concerns only four types of QNodes, described
below.  Eight more types of nodes have been defined in the research
literature and will be incorporated in future specifications
{{rdv-qce-arch}}.

QEndNode type definitions
=====================

MEAS
----

A MEAS node measures single-photon states sent to it.  It MUST be able
to measure in at least two bases, X and Z, either directly or by
applying a single-qubit gate before measurement.  It SHOULD be able to
switch between measurement bases on a per-measurement basis.

Successful measurement of the photon MAY be probabilistic, and most
likely will be low probability, when incorporating channel loss,
coupling loss, and detector efficiency.  Control systems and protocols
MUST be prepared for MEAS nodes to be probabilistic.

The measurement operation MAY be noisy, i.e. error-prone; this is the
expected case.  Link or device characterization processes such as
tomography will be used as part of network monitoring.  This
information MAY be used by network monitoring systems to declare a
link operational or non-operational, and MAY be used by a Responder to
plan RuleSets.

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

Relationship to multiplexing?

Multiplexing and resource reservation
------------

Multiplexing refers to the implementation of sharing of resources on a
link or path.  Resource reservation refers to policy decisions about
connection admission.  Because quantum states are owned by a Rule,
which belongs to a Connection, multiplexing is applied only to
BasePairs to assign them to a Connection.

Resource Reservation Protocol - Traffic Engineering (RSVP-TE) will be
adapted for connection admission and managing available link and path
resources {{RFC5151}}{{RFC6001}}.

Relationship to routing?

Challenge in starting connections with correct timing:

The problem is a ruleset execution starts at node A and node A tries
to send message to node B (while node B hasn't received/started the
ruleset) and the message gets dropped (potentially there can be a
mismatch in resource allocation as well when multiplexing is
concerned).

This is a design flaw, which should be addressed in the connection setup protocol itself.
Currently, once the RuleSet is received at a node. The node will try
to execute it right away. In certain instances, this will lead to the
node trying to send a message to another node while that node still
hasn't received the RuleSet. Thus, the message will not get recognized
at the destination and the node will discard/ignore that message. This
can lead to a certain type of deadlock behaviour.


Bipartite connection setup and teardown
-----------------------------

 (common portions)


White Rabbit absolute and relative clock synchronization
--------------------------------------------------------

Link characterization
---------------------

Each Link must be characterized using a bilateral process such as
tomography {{altepeter-tomography}}{{oka-qcit}}{{eisert-certification}}.

Quantum Router Software Architecture (QRSA)
===========================================

See Document 501 for an overview. To be adapted from existing QuISP documentation.

This document specifies node requirements in terms of externally
visible functionality, rather than mandating an implementation.  The
Quantum Router Software Architecture (QRSA) is the name of a reference
software implementation that conforms to this behavioral
specification, particularly the common node requirements described in
{{node-req}}.  The QRSA runs on each CNode that controls one or more
QNodes.

Subsets of the QRSA functionality are implemented in dfferent ways for
different node types.  Not all node types are required to implement
all functions.

The QRSA comprises the following five modules:

* routing daemon
* connection manager
* RuleEngine
* hardware monitor
* RealTime hardware drivers

All of these except the real-time hardware drivers communicate with
other CNodes.

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

Protocol timeouts and race conditions
============================

Security
========

QNode security
--------------------------

Protocol security
--------------------------

Network Monitoring
==================

Collection and reporting of performance statistics.

Link Monitoring
-----

Monitoring is first done at the link level, between neighboring
nodes.  Nodes MAY select their link quality and performance monitoring
algorithm or method independently of other links in the network.

Monitoring MAY be performed over more than a single hop, especially in
a switched link configuration.

The network monitoring system MAY declare a link up or down, for use
by Connections.

The following information MUST be reported by the link monitoring to
the routing subsystem, which will report it to the Connect Manager for
use in selecting paths and planning RuleSets:

* Entanglement attempt rate
* Entanglement success probability
* Fidelity of entanglement

Optional information that may be reported:

* X Error rate
* Z Error rate

Quantum Network Node Discovery
==============

Nodes will discover the presence and capabilities of neighbors via a
Quantum Network Node Discovery protocol.  Specification of this
protocol is out of scope for this document.

This protocol is designed for use by quantum nodes to discovery their
neighbors, rather than for non-quantum nodes to discover the presence
of quantum nodes.

--- back
