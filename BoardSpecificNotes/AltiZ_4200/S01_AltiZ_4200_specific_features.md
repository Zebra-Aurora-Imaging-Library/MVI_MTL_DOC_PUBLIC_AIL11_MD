---
doctype: BoardSpecificNotes
part: ""
chapter: AltiZ_4200
section: AltiZ_4200_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / altiz_4200 / AltiZ 4200 specific features"
---

# Zebra AltiZ 4200 overview

Zebra AltiZ 4200 is a family of 3D profile sensors that use camera-laser triangulation (line profiling) to acquire high-precision 3D data.

There are three models: AZ2S-2SB, AZ2S-3SB, and AZ2S-5SB. The primary specifications and notable differences between the Zebra AltiZ 4200 models are: clearance distance, working distance, available Z-range, X-axis field of view.

| Specifications[^wdist] | Zebra AltiZ 4200 family members |
| --- | --- |
| Clearance distance (min) | 38.13 mm | 79.67 mm | 82.37 mm |
| Working distance | 56.13 mm | 97.67 mm | 100.37 mm |
| Available Z-range | 20.16 mm | 44.53 mm | 69.42 mm |
| min Z - max Z | 56.13 mm - 76.29 mm | 97.67 mm - 142.20 mm | 100.37 mm - 169.79 mm |
| X-axis Field of View (near - far) | 26.22 mm - 32.34 mm | 43.25 mm - 58.84 mm | 69.28 mm - 93 mm |

[^wdist]: Note that these are approximate values and might slightly vary from unit to unit.

Zebra AltiZ 4200 supports the GenICam Standard Feature Naming Convention (SFNC) version 2.7 and the GenICam GenDC 1.0 specification with 3D datasets. It also supports custom GenICam features, which are outlined in the Zebra AltiZ 4200 Installation and Technical Reference. Zebra AltiZ 4200 can interface with a controlling computer and other devices over Ethernet (2.5GBASE-T) using the GigE Vision protocol. Zebra AltiZ 4200 can also interface with other devices using its auxiliary digital I/O lines (auxiliary I/O signals).

> **Note:** You can access the latest version of the Installation and Hardware Reference at [zebra.com/altiz-4200-info](https://zebra.com/altiz-4200-info). Click on the **Documentation** tab to access it.

## Summary of Zebra AltiZ 4200 features

The following table outlines the features currently available with each model of the Zebra AltiZ 4200.

| Features | Zebra AltiZ 4200 |
| --- | --- |
|  |
| Number of supported triggers | 4 general auxiliary |
| Asynchronous reset mode | Yes |
| Number of timers | 4 |
| Number of auxiliary signals (lines) | 6 (4 in and 2 out) |
| Quadrature decoder[^Quadrature] | 1 |

[^Quadrature]: Also known as quadrature encoder interface.
