---
doctype: BoardSpecificNotes
part: ""
chapter: Indio
section: Indio_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / indio-u53 / Indio specific features"
---

# Zebra Indio overview

Zebra Indio is a versatile, industrial I/O and communications, PCIe board. It has an Advanced I/O Engine that provides 16 optically-isolated, digital auxiliary I/O signals (8 inputs and 8 outputs), and includes I/O command lists, timers, quadrature decoders, and user outputs. It also supports GigE Vision capture, as well as Industrial communication using the PROFINET, EtherNet/IP, or Modbus/TCP industrial protocol. To support PROFINET communication, Zebra Indio also includes a PROFINET Engine.

In addition to being prelicensed for use with the Aurora Imaging Library drivers for GigE Vision cameras, the Zebra Indio board can act as a fingerprint for licensing supplemental Aurora Imaging Library functionality. The board can also store a supplemental Aurora Imaging Library license, which facilitates the process of moving to a new computer.

> **Note:** You can access the latest version of the Installation and Hardware Reference at [zebra.com/indio-info](https://zebra.com/indio-info).

## Summary of Zebra Indio features

The following table outlines the features currently available on Zebra Indio.

| Features | Zebra Indio |
| --- | --- |
| Can access GenICam camera features using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) / [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)[^GigE] | Yes |
| On-board memory | 128 MBytes DDR3 SDRAM |
| Number of acquisition ports | 1 |
| Number of auxiliary signals | 16 (8 in, 8 out) |
| PROFINET Engine | Yes |
| Number of timers | 16 |
| Number of quadrature decoders for input from linear or rotary encoders | 2 |
| Number of I/O command lists | 2 |
| Number of reference latches per I/O command list | 4 |
| Power-over-Ethernet (PoE) support | Yes |

[^GigE]: When using a GigE Vision system ([`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md), using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md)).
