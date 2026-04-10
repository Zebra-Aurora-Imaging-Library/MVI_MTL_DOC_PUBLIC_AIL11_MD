---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Finding_the_blob_location
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Finding the blob location"
---

# Finding the blob location and its bounding box

Finding the location of blobs in an image can sometimes be more useful than finding their shape or size. For example, if a robotic arm needs to pick up several items regardless of their type, it can use their location in an acquired image to determine their actual physical position.

You can also use the blob location to determine if a blob touches any side of the image border. If there are any such blobs, you might want to adjust the camera's field of view so that all items are completely represented in the image, or you might want to exclude these blobs.

## Points of the bounding box aligned with the pixel coordinate system

You can determine the blob's points of contact with the blob's bounding box aligned with the pixel coordinate system. To do so, enable the [`M_BOX`](../../Reference/blob/MblobControl.md), [`M_CONTACT_POINTS`](../../Reference/blob/MblobControl.md), and [`M_CENTER_OF_GRAVITY`](../../Reference/blob/MblobControl.md) groups of features for calculation using [`MblobControl`](../../Reference/blob/MblobControl.md). Use [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with the constants below to retrieve the corresponding point of contact after calculation.

Note that for the blob in the image below, there are at least two possible ways to determine each of its contact points. This is because there is only one point of contact on each side of the blob's border. For example:

*[Image: Cog3-labeled.png]*

| Legend |
| --- |
| A1 | ([`M_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md)) | A2 | ([`M_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md)) |
| B1 | ([`M_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md), [`M_Y_MIN_AT_X_MIN`](../../Reference/blob/MblobGetResult.md)) | B2 | ([`M_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_Y_MIN_AT_X_MAX`](../../Reference/blob/MblobGetResult.md)) |
| C1 | ([`M_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md), [`M_Y_MAX_AT_X_MIN`](../../Reference/blob/MblobGetResult.md)) | C2 | ([`M_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_Y_MAX_AT_X_MAX`](../../Reference/blob/MblobGetResult.md)) |
| D1 | ([`M_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md)) | D2 | ([`M_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md)) |
| E1 | ([`M_X_MIN_AT_Y_MIN`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md)) OR ([`M_FIRST_POINT_X`](../../Reference/blob/MblobGetResult.md), [`M_FIRST_POINT_Y`](../../Reference/blob/MblobGetResult.md)) | E2 | ([`M_X_MIN_AT_Y_MAX`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md)) |
| F1 | ([`M_X_MAX_AT_Y_MIN`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md)) | F2 | ([`M_X_MAX_AT_Y_MAX`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md)) |
| G | ([`M_CENTER_OF_GRAVITY_X`](../../Reference/blob/MblobGetResult.md), [`M_CENTER_OF_GRAVITY_Y`](../../Reference/blob/MblobGetResult.md)) |

In contrast, if there are multiple points of contact on one side of the blob border, each point of contact will be named differently, as shown in the image below.

*[Image: COG2.png]*

The center of gravity can be calculated in binary or grayscale mode. To calculate the latter, you must provide [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) with a grayscale image.

### Minimum-area and minimum-perimeter bounding box points

Besides points established from a blob's bounding box aligned with the pixel coordinate system, you can establish points from the minimum-area or the minimum-perimeter bounding box of the blob. To calculate the bounding box that has the least area, use the [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md) group of features; whereas to calculate the bounding box that has the smallest perimeter, use the [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md) group of features. These groups of features are enabled for calculation using [`MblobControl`](../../Reference/blob/MblobControl.md). Use [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with the following constants to retrieve them after calculation:

*[Image: MinAreaBoxOutline.png]*

| Area Box Legend | Perimeter Box Legend |
| --- | --- |
| A1 | ([`M_MIN_AREA_BOX_Xn`](../../Reference/blob/MblobGetResult.md), [`M_MIN_AREA_BOX_Yn`](../../Reference/blob/MblobGetResult.md))[^ValueOfn] | A2 | ([`M_MIN_PERIMETER_BOX_Xn`](../../Reference/blob/MblobGetResult.md), [`M_MIN_PERIMETER_BOX_Yn`](../../Reference/blob/MblobGetResult.md))[^ValueOfn] |
| B1 | ([`M_MIN_AREA_BOX_CENTER_X`](../../Reference/blob/MblobGetResult.md), [`M_MIN_AREA_BOX_CENTER_Y`](../../Reference/blob/MblobGetResult.md)) | B2 | ([`M_MIN_PERIMETER_BOX_CENTER_X`](../../Reference/blob/MblobGetResult.md), [`M_MIN_PERIMETER_BOX_CENTER_Y`](../../Reference/blob/MblobGetResult.md)) |
| C1 | [`M_MIN_AREA_BOX_WIDTH`](../../Reference/blob/MblobGetResult.md) | C2 | [`M_MIN_PERIMETER_BOX_WIDTH`](../../Reference/blob/MblobGetResult.md) |
| D1 | [`M_MIN_AREA_BOX_HEIGHT`](../../Reference/blob/MblobGetResult.md) | D2 | [`M_MIN_PERIMETER_BOX_HEIGHT`](../../Reference/blob/MblobGetResult.md) |
| E1 | [`M_MIN_AREA_BOX_ANGLE`](../../Reference/blob/MblobGetResult.md) | E2 | [`M_MIN_PERIMETER_BOX_ANGLE`](../../Reference/blob/MblobGetResult.md) |

[^ValueOfn]: n stands for an integer from 1 to 4.

> **Note:** Note that the width of the box is always the longer of the two sides. The angle of the minimum-area or minimum-perimeter bounding box is always measured from the X-axis to the side from which the width of the minimum-area or minimum-perimeter box is measured.

### Points with respect to the relative world coordinate system

When working with calibrated images, you can retrieve the above-mentioned points with respect to the relative coordinate system, by calling [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md) set to [`M_WORLD`](../../Reference/blob/MblobControl.md). However, in this case, the above-mentioned points are calculated in the pixel coordinate system and then the coordinates of these points are converted to the relative coordinate system.

To calculate the blob's bounding box and contact points in the relative coordinate system, you must enable the calculation of the [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md) group of features using [`MblobControl`](../../Reference/blob/MblobControl.md); then, use [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with the following constants to retrieve them after calculation:

*[Image: COG4.png]*

| Legend |
| --- |
|  |
| A1 | ([`M_WORLD_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md),[`M_WORLD_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md)) | A2 | ([`M_WORLD_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md)) |
| B1 | ([`M_WORLD_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md),[`M_WORLD_Y_AT_X_MIN`](../../Reference/blob/MblobGetResult.md)) | B2 | ([`M_WORLD_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_Y_AT_X_MAX`](../../Reference/blob/MblobGetResult.md)) |
| C1 | ([`M_WORLD_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md),[`M_WORLD_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md)) | C2 | ([`M_WORLD_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md)) |
| D1 | ([`M_WORLD_X_AT_Y_MIN`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md)) | D2 | ([`M_WORLD_X_AT_Y_MAX`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md)) |

## Chained pixels

You can obtain the coordinates of pixels bordering blobs or delimiting holes in blobs, in a counterclockwise or clockwise direction, respectively. These pixels are referred to as chained pixels. To calculate these pixels, enable the [`M_CHAINS`](../../Reference/blob/MblobControl.md) group of features for calculation using [`MblobControl`](../../Reference/blob/MblobControl.md).

You can use the chained pixel coordinates to create a chain code. A chain code is a directional code that records an object's boundary as a discrete set of vectors, where each vector points to the next pixel in the chain.

Chained pixels always form a closed chain. This implies that the starting pixel in the chain is also the closing one. If your blob has regions which are 1 pixel wide, these pixels are chained twice, once in the forward direction and then in the opposite direction.

In the diagram below, the thick lines illustrate pixels that are chained twice. The diagram also illustrates chained pixels of a blob in an 8 and 4-connected lattice, where the solid lines illustrate chained pixels in an 8-connected lattice, and the dotted lines illustrate how chained pixels deviate in a 4-connected lattice. Also, note that the blob's outermost chain is identified as index 1. Chains that delimit holes in blobs are identified by subsequent indices.

*[Image: chainpix.png]*

The [`M_CHAINS`](../../Reference/blob/MblobControl.md) feature group enables five separate chain features for calculation. This allows you to retrieve the following feature results using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md):

- [`M_TOTAL_NUMBER_OF_CHAINED_PIXELS`](../../Reference/blob/MblobGetResult.md), which retrieves the total number of chained pixels.
- [`M_NUMBER_OF_CHAINED_PIXELS`](../../Reference/blob/MblobGetResult.md), which retrieves the total number of chained pixels for each blob or a specified blob.
- [`M_CHAIN_INDEX`](../../Reference/blob/MblobGetResult.md), which assigns an index to each chained pixel, for every chain within a blob.
- [`M_CHAIN_X`](../../Reference/blob/MblobGetResult.md) and [`M_CHAIN_Y`](../../Reference/blob/MblobGetResult.md), which retrieves the X- and Y-coordinates of all chained pixels within a blob respectively.

When retrieving results for chain features, get the results for the number of chained pixels, in total or in a particular blob first ([`M_TOTAL_NUMBER_OF_CHAINED_PIXELS`](../../Reference/blob/MblobGetResult.md) or [`M_NUMBER_OF_CHAINED_PIXELS`](../../Reference/blob/MblobGetResult.md) respectively). This allows you to allocate an array that is large enough for the other chain results.

For the blob shown above, you would get the following results:

| Chain index | Chain-X | Chain-Y |
| --- | --- | --- |
| 1 | 4 | 2 |
| 1 | 4 | 3 |
| 1 | 4 | 4 |
| 1 | 4 | 5 |
| ... | ... | ... |
| 2 | 5 | 5 |
| 2 | 6 | 5 |
| 2 | 7 | 5 |
| 2 | 8 | 5 |

As mentioned, blobs with holes have multiple chains.

To determine the chain to which a pixel coordinate ([`M_CHAIN_X`](../../Reference/blob/MblobGetResult.md) and [`M_CHAIN_Y`](../../Reference/blob/MblobGetResult.md)) belongs, use [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_CHAIN_INDEX`](../../Reference/blob/MblobGetResult.md).
