---
doctype: BoardSpecificNotes
part: ""
chapter: Iris_GTX
section: Iris_GTX_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / iris-gtx / Iris GTX specific features"
---

# Zebra Iris GTX overview

Zebra Iris GTX is a family of smart cameras that combine image acquisition, embedded processing, and display. Zebra Iris GTX includes an Ethernet port (100/1000 BaseT), which provides connection for supported industrial protocols, including: PROFINET, EtherNet/IP, CC-Link, and Modbus. For PROFINET support, Zebra Iris GTX includes a PROFINET Engine; on Zebra Iris GTX, the PROFINET service comes preinstalled (but not enabled). Zebra Iris GTX also features an Advanced I/O Engine, which provides use of auxiliary I/O signals, an analog intensity control signal, general timers, an exposure timer, a strobe timer, a quadrature decoder, and an I/O command list (with 2 reference latches).

Zebra Iris GTX uses a CMOS image sensor, which supports an asynchronous externally triggered global shutter.

The primary difference between the members of the Zebra Iris GTX family are: the sensor chip size, the effective resolution, color, and frame rate.

| Zebra Iris GTX family members | Sensor chip size[^Chip_Size] | Effective resolution | Color/monochrome | Frame rate |
| --- | --- | --- | --- | --- |
| GTX2000 | 1/2.2"-type | 1920 x 1200 pixels | Monochrome | Up to 70.7 fps |
| GTX2000C | 1/2.2"-type | 1920 x 1200 pixels | Color | Up to 17.6 fps |
| GTX5000 | 2/3"-type | 2592 x 2048 pixels | Monochrome | Up to 41.7 fps |
| GTX5000C | 2/3"-type | 2592 x 2048 pixels | Color | Up to 10.4 fps |
| GTX8000 | 1/1.1"-type | 4096 x 2160 pixels | Monochrome | Up to 39.6 fps |
| GTX8000C | 1/1.1"-type | 4096 x 2160 pixels | Color | Up to 9.9 fps |
| GTX12000 | 1"-type | 4096 x 3072 pixels | Monochrome | Up to 28 fps |
| GTX12000C | 1"-type | 4096 x 3072 pixels | Color | Up to 7.0 fps |
| GTX16000 | 1.1"-type | 4000 x 4000 pixels | Monochrome | Up to 21.6 fps |
| GTX16000C | 1.1"-type | 4000 x 4000 pixels | Color | Up to 5.4 fps |

[^Chip_Size]: Sensor chips are always measured on the diagonal.

> **Note:** You can access the latest version of the Installation and Hardware Reference at [zebra.com/iris-gtx-info](https://zebra.com/iris-gtx-info). Click on the **Documentation** tab to access it.

## Summary of Zebra Iris GTX features

The following table outlines the features currently available on each member of the Zebra Iris GTX family.

| Features | Zebra Iris GTX |
| --- | --- |
| Can access GenICam camera features using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) / [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) | No |
| On-board memory | 4 Gbytes LPDDR4x SDRAM |
| Number of auxiliary signals | 7 (4 in, 3 out) |
| PROFINET Engine | Yes |
| Number of general timers | 8 |
| Number of strobe timers | 1 |
| Number of exposure timers | 1 |
| Number of quadrature decoders for input from linear or rotary encoders | 1 |
| Number of I/O command lists | 1 |
| Number of reference latches per I/O command list | 2 |
| Digitizer LUT | Yes |
| Asynchronous reset mode support | Yes |
