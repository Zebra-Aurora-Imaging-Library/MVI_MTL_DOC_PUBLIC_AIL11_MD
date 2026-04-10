---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Linescan_cameras
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Linescan cameras"
---

# Line-scan cameras

If your target digitizer supports it, you can grab from a line-scan camera as you would, for example, an RS-170 type camera. However, you should be aware of how data from these cameras is stored.

When acquiring data from a line-scan camera, each line (row) of each destination buffer band is filled from top to bottom. The operation will only end once the entire buffer has been filled.
