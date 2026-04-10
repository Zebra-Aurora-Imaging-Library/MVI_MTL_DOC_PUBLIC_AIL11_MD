---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_image_processing
section: Distance_transform
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-image / Distance transform"
---

# Distance transform

You can produce a distance transform using [`MimDistance`](../../Reference/im/MimDistance.md). This function determines the minimum distance from each foreground (non-zero) pixel to a background (zero) pixel, and assigns this distance to the foreground pixel. It produces a type of contour mapping of an image's foreground (object) pixels.

You can calculate the minimum distance using one of four transforms:

- City Block transform.
- Chessboard transform.
- Chamfer 3-4 transform.
- Euclidean transform.

## City Block transform

The City Block transform ([`M_CITY_BLOCK`](../../Reference/im/MimDistance.md)) determines the minimum distance using only horizontal or vertical steps. Each step counts as 1.

*[Image: cityblk.png]*

## Chessboard transform

The Chessboard transform ([`M_CHESSBOARD`](../../Reference/im/MimDistance.md)) determines the minimum distance using horizontal, vertical, or diagonal steps. Each step counts as 1.

*[Image: chessb.png]*

## Chamfer 3-4 transform

The Chamfer 3-4 transform ([`M_CHAMFER_3_4`](../../Reference/im/MimDistance.md)), like the Chessboard transform, determines the minimum distance using horizontal, vertical, or diagonal steps. However, horizontal and vertical steps are counted as 3 and diagonal steps as 4. This allows the transform to better approximate the true (Euclidean) distance between two pixels. However, it requires that the destination buffer be large enough to hold a number at least three times the maximum distance from a foreground to a background pixel. For example, an 8-bit buffer (255 max) can be used for a maximum distance of 85 pixels and a 16-bit buffer (65535 max) for a maximum distance of 21845 pixels.

*[Image: chamfer3.png]*

## Euclidean transform

The Euclidean transform ([`M_EUCLIDEAN_DIST`](../../Reference/im/MimDistance.md)) uses a straight-line path to determine the true minimum distance. The Euclidean distance is calculated using the following formula, where:

||
||
|  |
| *[Image: Euclidean_distance.png]* | - _x<sub>1</sub>_ and _y<sub>1</sub>_ represent the coordinates of the blob pixel.<br/>- _x<sub>2</sub>_ and _y<sub>2</sub>_ represent the coordinates of the nearest background pixel. |

Note, when using the Euclidean transform, the source buffer must be a 1-band, 8-bit image buffer of type [`M_UNSIGNED`](../../Reference/buf/MbufInquire.md) and the destination buffer must be a 1-band, 32-bit image buffer of type [`M_FLOAT`](../../Reference/buf/MbufInquire.md).
