---
doctype: BoardSpecificNotes
part: ""
chapter: AltiZ
section: AltiZ_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / altiz / AltiZ specific features"
---

# Zebra AltiZ overview

The Zebra AltiZ is a family of 3D profile sensors that use camera-laser triangulation to acquire high-precision 3D data. There are four models: AZ1D4SR, AZ1D4SB, AZ1D4MR, and AZ1D4LR.

The primary differences between the Zebra AltiZ models are: laser color and class, working distance, available Z-range, Z-axis resolution, and X-axis resolution.

| Specifications | Zebra AltiZ family members |
| --- | --- |
| Laser color and class | Red/2M | Blue/2M | Red/3R | Red/3R |
| Working distance | 100 mm | 100 mm | 185 mm | 160 mm |
| Available Z-range | 70 mm | 70 mm | 225 mm | 385 mm |
| Z-axis resolution | 4 µm - 8 µm | 4 µm - 8 µm | 9.5 µm - 35 µm | 10 µm - 89 µm |
| X-axis resolution | 39 µm | 39 µm | 84 µm | 158 µm |

Zebra AltiZ can interface with a controlling computer and other devices over Ethernet (1000Base-T) using the GigE Vision protocol. Zebra AltiZ can also interface with other devices using its auxiliary digital I/O lines (auxiliary I/O signals).

Zebra AltiZ supports the GenICam GenDC specification with 3D datasets. It also supports custom GenICam features, which are outlined in the Zebra AltiZ Installation and Technical Reference.

> **Note:** You can access the latest version of the Installation and Hardware Reference at [zebra.com/altiz-info](https://zebra.com/altiz-info). Click on the **Documentation** tab to access it.

## Summary of Zebra AltiZ features

The following table outlines the features currently available with each model of the Zebra AltiZ.

| Features | Zebra AltiZ |
| --- | --- |
| Number of supported triggers | 4 general auxiliary |
| Asynchronous reset mode | Yes |
| Number of timers | 4 |
| Number of auxiliary signals (lines) | 6 (4 in and 2 out) |
| Quadrature decoder[^Quadrature] | 1 |

[^Quadrature]: Also known as quadrature encoder interface.
