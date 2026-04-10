---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Features_related_to_depth_maps
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Features related to depth maps"
---

# Features related to depth maps

If you pass [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) a grayscale buffer that is a fully corrected depth map image buffer or 3D-processable depth map container, the function can calculate depth map related features for each blob, if the features are enabled for calculation. A depth map is an image where the grayscale value of a pixel represents its depth in the world, or a container where the range component stores the Z-coordinates, with X- and Y-coordinates stored implicitly by the point's column and row position in the pixel coordinate system, multiplied by X- and Y-scales. A depth map image is fully corrected if it accurately represents the height and shape of portrayed objects, and meets the following constraints:

- Calibrated image with a constant pixel size.
- [`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md) is not set to [`M_INVALID_SCALE`](../../Reference/cal/McalControl.md).

The image below shows a sample depth map.

*[Image: BlobDepthMap.png]*

You can verify that an image buffer contains a fully corrected depth map using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md). You can verify that a container contains a 3D-processable depth map using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md).

To ignore invalid pixels in the depth map, associate the image with a region of interest (ROI) that identifies the valid pixels. To do so, use [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with [`M_RASTERIZE_DEPTH_MAP_VALID_PIXELS`](../../Reference/buf/MbufSetRegion.md). You can also specify an image mask to further limit the ROI to pixels that are non-zero in the image mask buffer. For a depth map container, the ROI is stored in the [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufAllocComponent.md) component of the container.

## Features of depth maps

Before analyzing a depth map, you must enable the features that you want to calculate using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_DEPTH_MAP_...`](../../Reference/blob/MblobControl.md). You can analyze the following depth map features with the Blob Analysis module.

- The maximum elevation of the blob ([`M_DEPTH_MAP_MAX_ELEVATION`](../../Reference/blob/MblobGetResult.md)).
- The mean elevation of the blob ([`M_DEPTH_MAP_MEAN_ELEVATION`](../../Reference/blob/MblobGetResult.md)).
- The minimum elevation of the blob ([`M_DEPTH_MAP_MIN_ELEVATION`](../../Reference/blob/MblobGetResult.md)).
- The difference between the maximum and minimum elevations ([`M_DEPTH_MAP_SIZE_Z`](../../Reference/blob/MblobGetResult.md)).
- The volume of the blob ([`M_DEPTH_MAP_VOLUME`](../../Reference/blob/MblobGetResult.md)).

The image below shows the minimum and maximum elevation of the blob in the depth map above.

*[Image: blobdepthmap_sideways.png]*

## Examples

The _BlobDepthMap.cpp_ example analyzes the volumes of blobs in a sample depth map. To run this and other examples, use Aurora Imaging Example Launcher.
