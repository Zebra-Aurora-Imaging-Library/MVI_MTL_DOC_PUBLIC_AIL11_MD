---
doctype: BoardSpecificNotes
part: ""
chapter: GevIQ
section: GevIQ_overview
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / geviq / GevIQ overview"
---

# Zebra GevIQ overview

Zebra GevIQ is a smart network adapter that provides two 10 or 25 GbE interfaces (ports) for connection with 1/2.5/5/10/25 Gbits/sec GigE Vision-compatible cameras. Zebra GevIQ is a low-profile PCIe 3.0 x8 board that includes two SFP cages for connecting SFP+/SFP28 transceiver modules. The board can also be used as a standard network interface controller (NIC), and supports Aurora Imaging Library licensing fingerprint storage. Zebra GevIQ supports GigE Vision Standard version 2.2, GenICam GenApi version 3.4.2, GenICam Standard Feature Naming Convention (SFNC) version 2.7, and GenICam GenTL version 1.6.

Zebra GevIQ supports up to two GigE Vision cameras connected directly to the Zebra GevIQ board, or can offload up to 32 stream channels when connected behind a switch. Area-scan and line-scan monochrome or color video sources are supported, as long as they are GigE Vision-compliant (including video sources with a Bayer color filter).

GigE Vision streaming protocol packets are offloaded to Zebra GevIQ, where images are reconstructed on-board and then sent to the Host using scatter-gather DMA transfers if the destination buffer is in paged memory. Zebra GevIQ can send data to the Host at a maximum theoretical transfer rate of 8 Gbytes/sec using scatter-gather DMA transfers. Optimum conditions for high speed transfer include using the board in a PCIe 3.0 slot with 8 active lanes and using a 256-byte payload. If the destination buffer is non-paged, then standard DMA transfers are used.

To measure the effective available bandwidth of the PCIe slot in your computer with your Zebra GevIQ board, Zebra provides the Zebra GevIQ Bench utility. This utility is accessible using the Aurora Imaging Configurator utility. For more information on Zebra GevIQ Bench utility other tools, refer to [Zebra GevIQ utilities and tools](S03_GevIQ_utilities.md).

> **Note:** You can access the latest version of the Installation and Hardware Reference at [zebra.com/geviq-info](https://zebra.com/geviq-info).

## Summary of Zebra GevIQ features

The following table outlines the features currently available for Zebra GevIQ.

| Zebra GevIQ |
| --- |
| Can access GenICam camera features using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) / [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) | Yes |
| On-board memory | 4 Gbytes of DDR4 SDRAM |
| PCIe (3.0) board type | x8 low-profile board |
| Transceiver modules (SFP+/SFP28) | 2 |
| Number of stream channels | Up to 32 when connected behind a switch |
| Acquisition section | GigE Vision |
| GigE Vision compatible camera speeds | 1/2.5/5/10/25 Gbit/sec |
| Color space converter/Image formatter | Yes [^data_size] |
| Bayer decoder | Yes (GRBG, GBRG, BGGR, RGGB supported) |
| Supports Aurora Imaging Library licensing fingerprint storage | Yes |

[^data_size]: It can convert 8-bit or 16-bit monochrome, or 24-bit or 48-bit packed BGR data to monochrome, packed BGR, packed BGRa, planar RGB, or YUV (YUYV) format.
