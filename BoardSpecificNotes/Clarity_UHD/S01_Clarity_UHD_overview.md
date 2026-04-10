---
doctype: BoardSpecificNotes
part: ""
chapter: Clarity_UHD
section: Clarity_UHD_overview
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / clarity-UHD-u36 / Clarity UHD overview"
---

# Zebra Clarity UHD overview

Zebra Clarity UHD is a high-performance frame grabber, supporting standard definition (SD) analog video, high-definition (HD) analog and digital video, and ultra-high-definition (UHD) digital video input. Zebra Clarity UHD can be purchased with the additional feature of a H.264 encoder for video compression/decompression.

> **Note:** You can access the latest version of the Installation and Hardware Reference at [zebra.com/clarity-uhd-info](https://zebra.com/clarity-uhd-info).

## Acquisition features

Zebra Clarity UHD has eight independent acquisition paths which support the acquisition of up to 4 Gbytes (4096 Mbytes/sec) combined bandwidth from 10 possible connectors. The number of simultaneous grabs that a Zebra Clarity UHD can perform depends on the available connections and data formats used.

| Buffer pixel format | Maximum number of streams that can be grabbed and displayed |
| --- | --- |
| Monochrome 8-bit | 4 x 2160p60 streams and 4 x 1080p60 streams, or 6 x 2160p30 streams and 2 x 1080p60 streams, or 8 x 1080p60 streams |
| YUV 4:2:0 8-bit | 4 x 2160p60 streams, or 6 x 2160p30 streams and 2 x 1080p60 streams, or 8 x 1080p60 streams |
| YUV 4:2:2 8-bit | 3 x 2160p60 streams, or 6 x 2160p30 streams, or 8 x 1080p60 streams |
| YUV 4:2:2 10-bit | 2 x 2160p60 streams and one 1080p60 stream, or 4 x 2160p30 streams and one 1080p60 stream, or 8 x 1080p60 streams |
| RGB 8-bit planar or YUV 4:4:4 8-bit planar | 2 x 2160p60 streams, or 4 x 2160p30 streams, or 8 x 1080p60 streams |
| BGR32 8-bit packed | one 2160p60 stream and 2 x 1080p60 streams, or 3 x 2160p30 streams, or 6 x 1080p60 streams |
| RGB 10-bit packed | 3 x 2160p30 streams, or 6 x 1080p60 streams |

## Other features

Zebra Clarity UHD supports cropping (ROI capture), and subsampling (that is, downscaling) by integer factors of 1 to16.

When grabbing from an SDI source into a monochrome 8-bit, YUV16, or BGR32 packed buffer, Zebra Clarity UHD automatically converts the incoming data, using the proper color space equations, depending on the input resolution (ITU-R BT.601 for SD, ITU-R BT.709 for HD, and ITU-R BT.2020 for UHD). Note that, typically SDI streams are natively in YUV format. When grabbing into a YUV16 packed buffer, the input data is not converted.

If Zebra Clarity UHD has been purchased with the H.264 encoder option, the board can offload video compression/decompression from the Host, as well as perform video compression of the grabbed video input.

## Summary of Zebra Clarity UHD features

The following table outlines the features currently available on Zebra Clarity UHD.

| Features | Zebra Clarity UHD |
| --- | --- |
| PCIe board type (Form factor) | x8 half-length board |
| Acquisition format | Monochrome 8-bit, YUV16 (YCrCB) packed, and BGR32 packed |
| Number of acquisition paths | 8 independent |
| Number of supported hardware triggers | 0 |
| Number of auxiliary signals | 0 |
| On-board memory | 4 Gbytes of DDR3 SDRAM |
| Digitizer LUT | No |
| Number of display paths | 0 |
| Color space conversion specifications | ITU-R BT.601, ITU-R BT.709, ITU-R BT.2020 |
| Additional features | Formatting features (subsampling and cropping). Hooks for camera plug/unplug. Automatic input detection. Temperature and fan monitoring. |
| Optional features | H.264 video compression and decompression available on the Zebra Clarity UHD CLA4GHDSAE model |
