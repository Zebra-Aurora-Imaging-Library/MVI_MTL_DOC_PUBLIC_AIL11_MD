---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo-CoF
section: Rapixo-CoF_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cof / Rapixo-CoF specific features"
---

# Zebra Rapixo CoF overview

Zebra Rapixo CoF is a family of high-performance PCIe frame grabbers. There are two models:

- **Zebra Rapixo CoF40**. Zebra Rapixo CoF40 supports image capture from high-resolution and high-speed video sources that use the CoaXPress-over-Fiber (CoF) communication standard, offering a total aggregate bandwidth of up to 40 Gbps (4 x 10 Gbits/sec) for one or multiple standard CoaXPress links. A CoaXPress link contains all the connections and components needed to capture from one video source (camera). There are two versions of the Zebra Rapixo CoF40 model: one with 4GB of on board memory and one with 8GB of on board memory.
- **Zebra Rapixo CoF100**. Zebra Rapixo CoF100 supports the same functionality as Zebra Rapixo CoF40, offering a total aggregate bandwidth of up to 100 Gbps (4 x 25 Gbits/sec) across the CoaXPress links, as well as FPGA-based processing offload capabilities (Processing FPGA). This model comes with 16GB of on-board memory.

Zebra Rapixo CoF comes with one QSFP cage to which you must connect a QSFP+ or QSFP28 transceiver. The transceiver is used to receive video streams from 10 or 25 GbE interfaces that use the CoaXPress standard. In addition, this transceiver supports transmitting trigger signals and transmitting and receiving control acknowledge messages.

Zebra Rapixo CoF supports frame and line-scan, monochrome and color video sources. The color video sources can be RGB video sources or video sources with a Bayer color filter. Zebra Rapixo CoF can decode Bayer color-encoded images and perform color space conversions while transferring the images to the Host. In addition, it can vertically and horizontally flip them and perform subsampling.

## Summary of Zebra Rapixo CoF features

The following table outlines the features currently available for Zebra Rapixo CoF.

| Zebra Rapixo CoF |
| --- |
| Can access GenICam camera features using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) / [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) | Yes |
| On-board memory | 4 Gbytes DDR4 SDRAM or 8 Gbytes DDR4 SDRAM (Zebra Rapixo CoF40) 16 Gbytes DDR4 SDRAM (Zebra Rapixo CoF100) |
| Processing FPGA | No (Zebra Rapixo CoF40)/ Yes (Zebra Rapixo CoF100) |
| Digitizer LUTs | Yes [^data_size] |
| Frame burst technology | Yes |
| Flat-field correction | Yes[^flat_and_peak] |
| Peak-extraction | Yes[^flat_and_peak] |
| Acquisition section | CoaXPress |
| Number of acquisition paths | 4 |
| Number of auxiliary signals | 32 shared (16 in, 4 out, 12 in/out) |
| Number of timers | 8 shared |
| Number of quadrature decoders for input from linear or rotary encoders | 4 |
| Number of CoaXPress trigger signals (to camera only) | 1/digitizer |
| Number of data latches | 16/digitizer (allocated using [`M_DEV0`](../../Reference/dig/MdigAlloc.md) or [`M_DEV1`](../../Reference/dig/MdigAlloc.md) only) |

[^data_size]: Only for 8, 10, or 12-bit data (monochrome or color). When a link is receiving color data, all bands of the data use the same specified LUT mapping. In addition, as soon as one link is receiving 12-bit data, all links (CoaXPress connectors) share the same specified LUT mapping.

[^flat_and_peak]: Only available on Zebra Rapixo CoF100 (with the appropriate firmware selected). Not available when using Zebra Rapixo CoF100 with GenTL.

## Support features and standards

The Zebra Rapixo CoF driver supports the following standards:

- GenICam GenTL Standard version 1.6.
- GenICam GenApi version 3.4.2.
- GenICam Standard Feature Naming Convention (SFNC) version 2.7.
- JIIA CoaXPress Standard version 2.1.
- JIIA CoaXPress over Fiber Bridge Protocol version 1.1.

## Note on nomenclature

This manual refers to all versions of the board as Zebra Rapixo CoF. When necessary, this manual distinguishes between the boards using their full names (for example, Zebra Rapixo CoF40 or Zebra Rapixo CoF100), or their abbreviated forms (CoF40 and CoF100).
