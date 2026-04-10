---
doctype: BoardSpecificNotes
part: ""
chapter: Radient_eV-CL
section: Radient_eV-CL_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / radient_ev-cl / Radient eV-CL specific features"
---

# Zebra Radient eV-CL overview

Zebra Radient eV-CL is a family of high-performance PCIe frame grabbers:

- **Zebra Radient eV-CL**. Zebra Radient eV-CL supports image capture from up to 4 video sources that use the Camera Link (CL) communication standard, depending on the version. There are 7 versions of Zebra Radient eV-CL available: Zebra Radient eV-CL SB, Zebra Radient eV-CL x4 DB, Zebra Radient eV-CL x4 SF, Zebra Radient eV-CL x8 SF, Zebra Radient eV-CL x8 DB, Zebra Radient eV-CL DF, and Zebra Radient eV-CL QB.

Zebra Radient eV-CL supports frame and line-scan, monochrome and color video sources. The color video sources can be RGB video sources or video sources with a Bayer color filter. Zebra Radient eV can decode Bayer color-encoded images and perform color space conversions while transferring the images to the Host. In addition, it can vertically or horizontally flip them and perform subsampling.

Zebra Radient eV-CL supports up to 4 Base or 2 Full/80-bit mode Camera Link connections, depending on the version of the board, as well as providing Power-over-Camera Link (PoCL) support.

> **Note:** You can access the latest version of the Installation and Hardware Reference at [zebra.com/radient-ev-cl-info](https://zebra.com/radient-ev-cl-info).

## Summary of Zebra Radient eV-CL features

The following table outlines the features currently available for Zebra Radient eV-CL.

| Zebra Radient eV-CL |
| --- |
| Can access GenICam camera features using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) / [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) | Yes[^GenICam] |
| On-board memory [^_2] | 256 Mbytes DDR3 SDRAM (Zebra Radient eV-CL SB) 512 Mbytes DDR3 SDRAM (Zebra Radient eV-CL x4 DB) 512 Mbytes DDR3 SDRAM (Zebra Radient eV-CL x4 SF) 1 Gbytes DDR3 SDRAM (Zebra Radient eV-CL x8 SF) 1 Gbytes DDR3 SDRAM (Zebra Radient eV-CL x8 DB) 1 Gbytes DDR3 SDRAM (Zebra Radient eV-CL DF) 1 Gbytes DDR3 SDRAM (Zebra Radient eV-CL QB) |
| Processing FPGA | No |
| Digitizer LUTs | 8- and 10-bit (1/digitizer) or 12-bit (shared)[^_1] |
| Asynchronous camera reset support | Yes |
| Flat-field correction | Yes[^Flat] |
| Acquisition section | Camera Link |
| Number of acquisition paths | 4 (QB), 2 (DB, DF), 1 (SF, SB) |
| Number of auxiliary signals | 32 (4 in, 1 out, and 3 in/out per I/O connector) |
| Number of timers | 2/digitizer |
| Number of quadrature decoders for input from linear or rotary encoders | 1/digitizer |
| Number of supported triggers | 4/digitizer |
| Number of CoaXPress trigger signals | None |
| Number of UARTS | 1 |
| Number of data latches | 16/digitizer (allocated using [`M_DEV0`](../../Reference/dig/MdigAlloc.md) or [`M_DEV1`](../../Reference/dig/MdigAlloc.md) only)[^Flat_2] |

[^GenICam]: Only if the camera supports GenICam. For more information, refer to [Using GenICam with Camera Link cameras](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).

[^_2]: Zebra Radient eV has 128 Mbytes of acquisition memory mapped onto the PCIe bus. You can use a Host pointer to access this memory, or you can access it directly from another DMA capable PCIe device; this memory is referred to as shared memory.

[^_1]: When performing a color grab, one 12-bit LUT is used.

[^Flat]: Not supported on Zebra Radient eV-CL SB.

[^Flat_2]: Not supported on Zebra Radient eV-CL SB.

## Note on nomenclature

This manual refers to all versions of the board as Zebra Radient eV-CL. Zebra Radient eV-CL refers to Zebra Radient eV-CL SB, Zebra Radient eV-CL SF, Zebra Radient eV-CL DF, Zebra Radient eV-CL DB, and Zebra Radient eV-CL QB versions. When necessary, this manual distinguishes between the Zebra products using their full names.
