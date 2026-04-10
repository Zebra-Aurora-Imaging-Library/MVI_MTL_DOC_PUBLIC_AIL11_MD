---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_image_processing
section: Connectivity_mapping
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-image / Connectivity mapping"
---

# Connectivity mapping

In some cases, you might need to isolate specific points of a binary, skeletonized image (for example, end points or points of intersection). You could perform several morphological processing steps, where each step is performed using a different structuring element; however, the result is a time-consuming serial operation. The connectivity (or cellular) mapping function, [`MimConnectMap`](../../Reference/im/MimConnectMap.md) can reduce this serial operation into a parallel one, increasing efficiency.

Specifically, treating a source image as if it were binary, [`MimConnectMap`](../../Reference/im/MimConnectMap.md) concatenates the binary values of the pixels in each pixel's 3x3 neighborhood and maps this 9-bit value, referred to as a connectivity code, through a custom LUT. Assuming you program the custom LUT with the values that would result if the required structuring elements were applied to the source image consecutively, you essentially reduce the serial operation to a parallel one. Since each connectivity code has 9 bits, the process requires a LUT buffer with at least 512 (that is, `2<sup>9</sup>`) entries.

## Using the connectivity code

The connectivity code gives you the possibility to explicitly choose the destination value for any 3x3 source pixel configuration (pattern), with the connectivity code being the address of a position within the custom LUT.

The connectivity code is obtained by concatenating the binary values of a pixel's 3x3 neighborhood into a single 9-bit number; all non-zero pixels are treated as 1. Neighborhood pixels are linked in the following order.

*[Image: connectivitycode.png]*

The pixels are connected and mapped as follows:

*[Image: ConnectivityCodeFormula.png]*

`_Result_ = LUTMAP[_Connectivity code_]`

The pixel configuration in the neighborhood of pixel `_n_<sub>8</sub>` (including pixel `_n_<sub>8</sub>`) forms a binary number between 0 and 511 inclusively (with `_n_<sub>8</sub>` being the most significant bit and `_n_<sub>0</sub>` being the least significant bit). This 9-bit number is then used to address the user-supplied LUT that contains the value to be written to the destination buffer.

For example, the 3x3 neighborhood source configuration *[Image: Sigma_example.png]*

produces the following binary number: 111100100 (or 484). The destination buffer for this position receives the value of the user-supplied LUT at position 484.

## Locating different types of points

There are several different types of points that connectivity mapping can locate.

- **Isolated points**. Single points that are stand alone. The connectivity code for the 3x3 neighborhood of an isolated point is 100000000.
  *[Image: conn_code_isolated_point.png]*
- **End points**. Points that represent the end of a line segment. The connectivity code for the 3x3 neighborhood of two possible end points are 100001000 and 100000100.
  *[Image: conn_code_end_point.png]*
- **Triple points**. Points of intersection which occur at the common junction of 3 line segments. The connectivity code for the 3x3 neighborhood of two possible triple points are 101001010 and 101010100.
  *[Image: conn_code_triple_point.png]*
- **Cross points**. Points of intersection which occur at the common junction of 4 line segments. The connectivity code for the 3x3 neighborhood of two possible cross points are 110101010 and 101010101.
  *[Image: conn_code_cross_point.png]*

## Example

In the following example, the image on the left (an example of a circuit) is converted into a skeletonized image, and then the triple points and end points are located and displayed.

*[Image: connectivity_corona2.png]*
