---
doctype: BoardSpecificNotes
part: ""
chapter: Concord_PoE
section: Concord_POE_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / Concord_PoE / Concord POE specific features"
---

# Zebra Concord PoE overview

The Zebra Concord PoE family consists of two models: the Zebra Concord PoE base model and Zebra Concord PoE with ToE.

Zebra Concord PoE base model is a Gigabit Ethernet network interface board that provides optimum support for interfacing with GigE Vision cameras and supplies Power-over-Ethernet (PoE) to GigE Vision cameras that support PoE. There are two versions of the Zebra Concord PoE base model: the Dual-port version and the Quad-port version.

Zebra Concord PoE with ToE includes all the features of the Zebra Concord PoE base model, with the addition of an Advanced I/O Engine. The Advanced I/O Engine provides use of 8 auxiliary signals, and includes I/O command lists, timers, quadrature decoders, user outputs, and a Trigger-over-Ethernet (ToE) module. The Trigger-over-Ethernet module can transmit a Trigger-over-Ethernet packet, either as an action command or a GigE Vision software trigger, upon reception of an internal or external event. There are two versions of Zebra Concord PoE with ToE: the Dual-port version and the Quad-port version.

In addition to being prelicensed for use with the Aurora Imaging Library drivers for GigE Vision cameras, the Zebra Concord PoE board can act as a fingerprint for licensing supplemental Aurora Imaging Library functionality. The board can also store a supplemental Aurora Imaging Library license, which facilitates the process of moving to a new computer.

> **Note:** You can access the latest version of the Installation and Hardware Reference at [zebra.com/concord-poe-info](https://zebra.com/concord-poe-info).

## Summary of Zebra Concord PoE features

The following table outlines the features currently available for Zebra Concord PoE family.

| Zebra Concord PoE base model | Zebra Concord PoE with ToE |
| --- | --- |
| Can access GenICam camera features using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) / [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)[^GigE] | Yes | Yes |
| On-board memory | N/A | N/A |
| Number of acquisition ports | 2 (Dual) or 4 (Quad) | 2 (Dual) or 4 (Quad) |
| Number of auxiliary signals | 0 | 8 (6 in, 2 out) |
| Trigger-over-Ethernet (ToE) module | No | Yes |
| Number of timers | 0 | 16 |
| Number of quadrature decoders for input from linear or rotary encoders | 0 | 2 |
| Number of I/O command lists | 0 | 2 |
| Number of reference latches per I/O command list | 0 | 4 |
| Power over Ethernet (PoE) support | Yes | Yes |

[^GigE]: When using a GigE Vision system ([`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md), using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md)).

## Note on nomenclature

This manual refers to all models of Zebra Concord PoE as the Zebra Concord PoE. When necessary, this manual distinguishes between the boards using their full names (Zebra Concord PoE base model and Zebra Concord PoE with ToE).
