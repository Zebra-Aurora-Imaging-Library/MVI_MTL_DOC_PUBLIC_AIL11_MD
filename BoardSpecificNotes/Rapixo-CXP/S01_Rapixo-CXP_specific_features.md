---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo-CXP
section: Rapixo-CXP_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cxp / Rapixo-CXP specific features"
---

# Zebra Rapixo CXP overview

Zebra Rapixo CXP is a family of high-performance PCIe frame grabbers. There are two models:

- **Zebra Rapixo CXP base model**. Zebra Rapixo CXP base model supports image capture from up to 4 high-resolution, high-speed video sources that use the CoaXPress (CXP) communication standard. There are five versions of the Zebra Rapixo CXP base model: Single, Dual, Quad CXP-6, Quad CXP-12, and Quad Data Forwarding.
- **Zebra Rapixo CXP Pro**. Zebra Rapixo CXP Pro supports the same functionality as the base model, as well as FPGA-based processing offload capabilities (Processing FPGA). There is only one version of this model: Pro Quad CXP-12.

Zebra Rapixo CXP supports frame and line-scan, monochrome and color video sources. The color video sources can be RGB video sources or video sources with a Bayer color filter. Zebra Rapixo CXP can decode Bayer color-encoded images and perform color space conversions while transferring the images to the Host. In addition, it can vertically and horizontally flip them and perform subsampling.

Zebra Rapixo CXP can receive images from each video source at up to the maximum CXP-6 or CXP-12 speed (depending on the model of the board and camera). Using link aggregation, the bandwidth of a link can be increased to 4 times the maximum CXP-6 and CXP-12 speeds for a single video source. Zebra Rapixo CXP provides up to 13 W of power per CoaXPress connection to any device that supports power-over-CoaXPress (PoCXP).

> **Note:** You can access the latest version of the Installation and Hardware Reference at [zebra.com/rapixo-cxp-info](https://zebra.com/rapixo-cxp-info).

## Summary of Zebra Rapixo CXP features

The following table outlines the features currently available for Zebra Rapixo CXP.

| Zebra Rapixo CXP |
| --- |
| Can access GenICam camera features using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) / [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) | Yes |
| On-board memory | 1 Gbytes DDR3 SDRAM (Zebra Rapixo CXP Single base model) 2 Gbytes DDR3 SDRAM (Zebra Rapixo CXP Dual base model) 4 Gbytes DDR4 SDRAM (Zebra Rapixo CXP Quad CXP-6/CXP-12 base model) 4 Gbytes DDR4 SDRAM (Zebra Rapixo CXP Quad Data Forwarding base model) 8 Gbytes DDR4 SDRAM (Zebra Rapixo CXP Pro Quad CXP-12) |
| Processing FPGA | No (Zebra Rapixo CXP base model)/ Yes (Zebra Rapixo CXP Pro Quad CXP-12) |
| Digitizer LUTs | Yes [^data_size] |
| Frame burst technology | Yes |
| Flat-field correction | Yes[^flat] |
| Peak-extraction | Yes[^peak] |
| Acquisition section | CoaXPress |
| Number of acquisition paths | 4 |
| Number of auxiliary signals | 32 shared (16 in, 4 out, 12 in/out) |
| Number of timers | 4 shared |
| Number of quadrature decoders for input from linear or rotary encoders | 4 |
| Number of CoaXPress trigger signals (to camera only) | 1/digitizer |
| Number of data latches | 16/digitizer (allocated using [`M_DEV0`](../../Reference/dig/MdigAlloc.md) or [`M_DEV1`](../../Reference/dig/MdigAlloc.md) only) |

[^data_size]: Only for 8, 10, or 12-bit data (monochrome or color). When a link is receiving color data, all bands of the data use the same specified LUT mapping. In addition, as soon as one link is receiving 12-bit data, all links (CoaXPress connectors) share the same specified LUT mapping.

[^flat]: Only available on Zebra Rapixo CXP Pro Quad CXP-12 (with the appropriate firmware selected). Not available when using Zebra Rapixo CXP Pro Quad CXP-12 with GenTL.

[^peak]: Only available on Zebra Rapixo CXP Pro Quad CXP-12 (with the appropriate firmware selected). Not available when using Zebra Rapixo CXP Pro Quad CXP-12 with GenTL.

## Note on nomenclature

This manual refers to all versions of the board as Zebra Rapixo CXP. When necessary, this manual distinguishes between the boards using their full names (for example, Zebra Rapixo CXP Quad CXP-6, Zebra Rapixo CXP Quad CXP-12, or Zebra Rapixo CXP Pro Quad CXP-12), or their abbreviated forms (Quad CXP-6, Quad CXP-12, and Pro Quad CXP-12).
