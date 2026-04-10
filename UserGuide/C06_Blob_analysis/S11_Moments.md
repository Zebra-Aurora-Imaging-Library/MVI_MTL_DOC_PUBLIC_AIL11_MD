---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Moments
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Moments"
---

# Moments

Using the Blob Analysis module, you can also calculate the moments used to find the center of gravity, as well as other grayscale and binary moments. To calculate a moment of a specified order in X and Y (general moment), enable the [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md) feature for calculation using [`MblobControl`](../../Reference/blob/MblobControl.md); then specify the X and Y order of the moment to calculate using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_GENERAL_ORDER_X`](../../Reference/blob/MblobControl.md) and [`M_MOMENT_GENERAL_ORDER_Y`](../../Reference/blob/MblobControl.md), respectively. Select whether to calculate the ordinary or the central moment using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_GENERAL_MODE`](../../Reference/blob/MblobControl.md). Central moments use coordinates that are relative to the center of gravity of the blob, and therefore are independent of a blob's position within the image; whereas, ordinary moments are affected by the blob position because they use coordinates relative to the top-left corner of the image. Use [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MOMENT_GENERAL`](../../Reference/blob/MblobGetResult.md) to retrieve the result.

You could alternatively enable the calculation of the [`M_MOMENT_FIRST_ORDER`](../../Reference/blob/MblobControl.md), [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md) and/or [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md) group of features for calculation using [`MblobControl`](../../Reference/blob/MblobControl.md). These groups of features include only the more common moments (for example, [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MOMENT_X0_Y1`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_X0_Y2`](../../Reference/blob/MblobGetResult.md) or [`M_MOMENT_X0_Y3`](../../Reference/blob/MblobGetResult.md)). These common moments are calculated faster than the general moment.

You could also enable the calculation of hu moment invariants using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md). Hu moments are a set of seven moments calculated using central moments. Hu moments are invariant to translation, rotation and scaling.
