---
title: Quantum Measurement Network Node (MEAS)
abbrev: QMEAS
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

This specifies the behavior of the measurement (MEAS) quantum network node.

--- middle

Introduction        {#intro}
============

A measurement node (MEAS) measures single-photon states received from
an optical Channel.

MEAS QEndNode type definition
=====================

A MEAS node measures single-photon states sent to it.  It MUST be able
to measure in at least two bases, X and Z, either directly or by
applying a single-qubit gate before measurement.  It SHOULD be able to
switch between measurement bases on a per-measurement basis.  Basis
selection may be either passive, or under software/hardware control.

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

A MEAS MAY consist of more than one physical photon detector treated
as a single unit, in order to detect two eigenstates of the same
measurement basis or to measure in two bases.

Active v. Passive Basis Selection
-----

Depending on the implementation of the quantum plane of the MEAS node,
the measurement basis for individual operations MAY be either Active,
under hardware and software control, or Passive, selected at random by
a physical process such as the use of a beam splitter.

For entangled photon pairs where both photons are being measured at
independent MEAS nodes, the two nodes MAY differ in this respect.  One
node may be Passive and the other Active.

WavePackets and TimingWindows
=====

PHY Synchronization
=====

Synchronization of MEAS involves two major tasks:

* coordinating the enabling and disabling of the physical detector
  with the chosen DetectionWindow such that it overlaps with the
  WavePacket
* identifying the WavePacketTrain and the WavePacket such that events
  are properly identified and reported to local software and/or the
  photon transmitting nodes

Timing Negotiation
=====

MEAS nodes MUST negotiate timing with nodes that transmit the photons
to be measured.

Parameters
======

Fixed
-----

The following parameters are implicit in the type of hardware selected
and deployed.

* Wavelength envelope
* Accepted WavePacket format
* PassiveActiveBasis

Configured
-----

* *Minimum inter-photon interval* (MIPI): the minimum time between two
  consecutive photon WavePackets in a WavePacketTrain.  Units are in
  nanoseconds.  Specified as a floating point number.
* Maximum WavePacketTrain length: the maximum number of WavePackets in
  a single train.
* Minimum inter-train interval: the minimum time between the end of
  one WavePacketTrain, including the outer limits of the WavePacket
  for the last photon, and the arrival of the Sync signal for the next
  WavePacketTrain.
* PhySyncOffset: the interval between the arrival of the physical
  synchronization signal and the start of the first detector window.
  Units are in nanoseconds.  Specified as a floating point number.

Measured/Monitored
-----

* XMeasurementDroppedQubit
* ZMeasurementDroppedQubit
* XMeasurementDarkCount
* ZMeasurementDarkCount
* XasZMeasurementError
* ZasXMeasurementError


--- back
