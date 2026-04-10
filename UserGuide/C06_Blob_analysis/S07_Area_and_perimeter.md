---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Area_and_perimeter
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Area and perimeter"
---

# Area and perimeter

Each pixel in your image represents a real width and height (for example, in millimeters). However, [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) performs calculations and measurements (that represent a distance or area) in raw (uncalibrated) pixel units. By default, [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) assumes that the width and height of the pixels are the same (that is, the pixel aspect ratio (width/height) equals 1.0); therefore, each pixel (P) is represented as follows:

*[Image: Area.png]*

A pixel ratio of 1.0 implies that the retrieved area ([`M_AREA`](../../Reference/blob/MblobGetResult.md)) of a single pixel blob is equal to 1 and the retrieved perimeter ([`M_PERIMETER`](../../Reference/blob/MblobGetResult.md)) is equal to 4. When calculating the area and perimeter of a larger blob, the area would then equal the number of pixels in the blob (excluding holes), and the perimeter would equal the total number of pixel sides along the blob edges (including the edges of holes). Note, an allowance is made for the staircase effect that occurs in a digital image when representing diagonals and curves. For example, in the following blob (where F represents foreground pixels), the area is 10 and the perimeter is 14.242.

*[Image: Area2.png]*

When two diagonals intersect, as in the image below, the perimeter is calculated as the sum of the straight edges and of the diagonals (calculated as described above) with a correction of `-1` for each intersection of diagonals. For example, in the following blob, the area is 11 and the perimeter is 17.656.

*[Image: Area3.png]*

If what is of more interest is the smallest area or smallest perimeter of the rectangular region that a blob occupies (bounding box), you can enable for calculation the [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md) and [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md) group of features, respectively, using [`MblobControl`](../../Reference/blob/MblobControl.md). For more information on bounding boxes, see [Finding the blob location and its bounding box](S10_Finding_the_blob_location.md).

You can also calculate an exact calculation, or an approximation, of the convex perimeter of the blobs using [`M_CONVEX_HULL_PERIMETER`](../../Reference/blob/MblobGetResult.md) or [`M_CONVEX_PERIMETER`](../../Reference/blob/MblobGetResult.md) respectively. These are enabled for calculation using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md) and [`M_CONVEX_PERIMETER`](../../Reference/blob/MblobControl.md) respectively. The convex perimeter is the perimeter of the convex hull (see below).

*[Image: Boltarea.png]*

The approximation of the convex perimeter ([`M_CONVEX_PERIMETER`](../../Reference/blob/MblobGetResult.md)) is derived by taking the diameter of the blob (Feret diameter) at different angles. You can adjust the number of Feret diameters used for the calculation using the [`MblobControl`](../../Reference/blob/MblobControl.md) function with [`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md). The greater the number of Feret diameters used, the more accurate the approximation.

Note that the Feret diameters are calculated using a method which treats pixels as being round with a diameter of 1 instead of being square (see [Dimensions](S08_Dimensions.md)). This can lead to results not matching the expected result. For example, when dealing with a single pixel blob, the result approaches 3.1416 (that is, Pi) which is the perimeter of a circle with a diameter of 1.

When [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_PIXEL_ASPECT_RATIO`](../../Reference/blob/MblobControl.md) has been set to anything other than 1.0, the aspect ratio is applied to the pixel width during calculations. Each pixel is now represented as:

*[Image: aspect2.png]*

This affects all calculated features as if you had actually stretched the image (from the top-left corner in the X-direction only), by a factor equal to the pixel aspect ratio. We can no longer say that results are in "pixel" units. In fact, results are really in units of "pixel height" since the height is not affected by the aspect ratio.
