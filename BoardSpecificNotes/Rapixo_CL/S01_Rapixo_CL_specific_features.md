---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo_CL
section: Rapixo_CL_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cl-pro / Rapixo CL specific features"
---

# Zebra Rapixo CL overview

Zebra Rapixo CL is a family of high-performance PCIe frame grabbers. There are two models:

- **Zebra Rapixo CL base model**. Zebra Rapixo CL base model supports image capture from up to 2 high-resolution, high-speed video sources that use the Camera Link (CL) communication standard. There are two versions of this model: Zebra Rapixo CL DB and Zebra Rapixo CL SF.
  Zebra Rapixo CL DB supports acquisition from two Camera Link video sources in Base configuration, and Zebra Rapixo CL SF supports acquisition from one Camera Link video source in Medium, Full, 72-bit, or 80-bit configuration.
- **Zebra Rapixo CL Pro**. Zebra Rapixo CXP Pro supports the same functionality as the base model, as well as FPGA-based processing offload capabilities (Processing FPGA). There are five versions of this model: Zebra Rapixo CL Pro SF, Zebra Rapixo CL Pro DB, Zebra Rapixo CL Pro QB, and Zebra Rapixo CL Pro DF.
  Zebra Rapixo CL Pro DB supports acquisition from two Camera Link video sources in Base configuration; Zebra Rapixo CL Pro QB supports acquisition from four of these video sources. Zebra Rapixo CL Pro SF supports acquisition from one Camera Link video source in Medium, Full, 72-bit, or 80-bit configuration; Zebra Rapixo CL Pro DF supports two of these sources.

Zebra Rapixo CL supports area-scan and line-scan monochrome and color video sources. The color video sources can be RGB video sources or video sources with a Bayer color filter. Zebra Rapixo CL can decode Bayer color-encoded images and perform color space conversions while transferring the image to the Host. Besides standard Camera Link video sources, Zebra Rapixo CL also supports additional types of video sources, including some time-multiplexed video sources.

As a Zebra Rapixo CL grabs images, it can vertically or horizontally flip them. In addition, Zebra Rapixo CL can perform color-space conversions, Bayer decoding, and image subsampling.

> **Note:** You can access the latest version of the Zebra Rapixo CL and Rapixo CL Pro Installation and Hardware Reference at [zebra.com/rapixo-cl-info](https://zebra.com/rapixo-cl-info) and [zebra.com/rapixo-cl-pro-info](https://zebra.com/rapixo-cl-pro-info), respectively.

## Summary of Zebra Rapixo CL and CL Pro features

The following table outlines the features currently available for Zebra Rapixo CL.

| Zebra Rapixo CL |
| --- |
| Can access GenICam camera features using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) / [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)[^GenICam] | Yes |
| On-board memory | 512 Mbytes DDR3 SDRAM (Zebra Rapixo CL DB) 512 Mbytes DDR3 SDRAM (Zebra Rapixo CL SF) 4 Gbytes DDR3 SDRAM (Zebra Rapixo CL Pro) |
| Processing FPGA | No (Zebra Rapixo CL base model)/ Yes (Zebra Rapixo CL Pro) |
| Digitizer LUTs | 8- and 10-bit (1/digitizer) or 12-bit (shared) [^colorgrab] |
| Asynchronous camera reset support | Yes |
| Acquisition section | Camera Link |
| Number of acquisition paths | 4 (QB), 2 (DB, DF), 1 (SF) |
| Number of supported triggers | 7/Digitizer |
| Number of timers | 2/Digitizer |
| Number of UARTS | 1 per CL connector |
| Number of auxiliary signals | 32 (4 in, 1 out, and 3 in/out per I/O connector) |
| Number of quadrature decoders | 4 |
| Power-over-Camera Link (PoCL) support | Yes |
| Number of data latches | 16 only available on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (Zebra Rapixo CL) 16/Digitizer (Zebra Rapixo CL Pro) |

[^GenICam]: Only if the camera supports GenICam. For more information, refer to [Using GenICam with Camera Link cameras](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).

[^colorgrab]: When performing a color grab, one 12-bit LUT is used.

## Note on nomenclature

This manual refers to all versions of the Zebra Rapixo CL family of frame grabbers as Zebra Rapixo CL and Zebra Rapixo CL **Pro** if an on-board processing FPGA is present. When necessary, this manual distinguishes between the Zebra Rapixo CL SF, Zebra Rapixo CL DB, Zebra Rapixo CL Pro SF, Zebra Rapixo CL Pro DB, Zebra Rapixo CL Pro QB, Zebra Rapixo CL Pro DF using their full names.
