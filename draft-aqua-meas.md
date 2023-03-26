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

This specifies the behavior of the measurement (MEAS) quantum network node.

--- middle

Introduction        {#intro}
============

--- back
