---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Metrology
section: Calculate_volume
module_tag: 3dmet
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Metrology / Calculate volume"
---

# Calculating a volume

You can calculate the volume of a point cloud's mesh or a fully corrected depth map, using [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) or [`M3dmetVolume`](../../Reference/3dmet/M3dmetVolume.md). You can, and in some cases must, specify a reference object to delimit the volume. Note that [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) stores the result of the volume operation in a result buffer. [`M3dmetVolume`](../../Reference/3dmet/M3dmetVolume.md) does not require a result buffer and returns the calculated volume directly.

## Specifying a reference object

When calculating the volume of a mesh without holes, you do not need to specify a reference object. When calculating the volume of a depth map (with or without holes) or a mesh that has holes, a reference object is required to define a closed shape for which to calculate the volume. In either case, you might need to specify a reference object to limit the volume to calculate.

> **Note:** Note that to reduce the number of holes, you can try using the smoothed reconstruction mode ([`M_MESH_SMOOTHED`](../../Reference/3dim/M3dimControl.md)) to generate the mesh. For more information, see [Surface reconstruction modes](../C35_3D_Image_processing/S10_Generating_a_mesh_for_a_point_cloud.md).

You can specify that the reference is the XY (Z=0) plane, a defined 3D plane geometry, or a fully corrected depth map (when the source is also a depth map). By default, for a depth map as a source, the reference is the XY (Z=0) plane; however, for a mesh, there is no default reference (if the mesh has holes, you must explicitly pass a reference object).

When you specify a reference object and the source is a depth map or a mesh with holes, [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) or [`M3dmetVolume`](../../Reference/3dmet/M3dmetVolume.md) extends the boundaries of the source object onto it, to define the closed 3D shape for which to calculate the volume. If the source is a mesh, its boundaries are extended perpendicular to the reference object. If the source is a depth map image buffer, its boundaries are extended in the Z-direction until they meet the reference object.

### Delimiting the volume to calculate

Whether or not a reference object is required, you can specify one to limit the volume to calculate. For example, you can calculate just the volume above the reference object ([`M_ABOVE`](../../Reference/3dmet/M3dmetVolume.md)), under the reference object ([`M_UNDER`](../../Reference/3dmet/M3dmetVolume.md)), or the volume that is above the reference object minus the volume below it ([`M_DIFFERENCE`](../../Reference/3dmet/M3dmetVolume.md)).

These calculations are made, given the position of the source object relative to the reference object. If the reference object is a plane, any volume on the same side as the plane's normal is considered to be above the plane. If the reference object is a depth map, a given pixel in the source depth map is considered above a corresponding pixel (at the same X- and Y-coordinate) in the reference depth map if its Z-coordinate is greater.

By default, [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) and [`M3dmetVolume`](../../Reference/3dmet/M3dmetVolume.md) calculate the sum of all volumes between the source and reference objects, whether above or below the reference object.

### Considerations

If the source is a depth map with holes, you can use [`M3dimFillGaps`](../../Reference/3dim/M3dimFillGaps.md) to fill them before calculating the volume.

If the source is a mesh with disconnected triangles, the volumes for each set of connected triangles are calculated separately and summed in the final result. This can lead to unexpected results if two sets of triangles overlap, or if one set of triangles is above another with respect to the reference object.

### Volume elements

A volume element is a triangular prism or pyramid that represents the computed 3D volume associated with one triangle of the source meshed point cloud. Pyramids occur for closed mesh volume calculations, since, in this case, the volume element is computed between a surface triangle and the origin of the working coordinate system. For a source depth map, the volume element is a rectangular prism.

For more information on spliced volume elements, see [](S06_Calculate_volume.md).

## How the volume of a meshed point cloud with holes is calculated

The following steps describe how the volume of a mesh with holes is calculated.

When calculating the volume of a mesh with holes, it is the orientation of the meshed point cloud's triangles that defines how [`M3dmetVolume`](../../Reference/3dmet/M3dmetVolume.md) calculates the volume.

*[Image: 3dmetVolume_mesh_large.png]*

1. An arbitrary orientation (relative to the reference plane) is guessed for one triangle.
   *[Image: 3dmetVolume_arrow_one_large.png]*
2. The orientation of each triangle in the mesh is extrapolated from the first triangle.
   *[Image: 3dmetVolume_arrow_all_bottom_large.png]*
   > **Note:** If any triangle in the mesh crosses the reference plane, the triangle is subdivided into triangles that do not.
3. For each triangle above the reference plane, a triangular prism (volume element) from the triangle to the plane is defined.
4. If the triangle was orientated away from the reference plane, the volume of the prism adds to the total calculated volume (illustrated in green). If the triangle was orientated towards the reference plane, the volume of the prism subtracts from the total calculated volume (illustrated in red).
   *[Image: 3dmetVolume_prism_large.png]*
5. The absolute value of the total calculated volume is returned.

After a successful volume computation, you can use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) to obtain the calculated volume and other data, such as the number of volume elements that were used in the calculation ([`M_VOLUME_NB_ELEMENTS`](../../Reference/3dmet/M3dmetGetResult.md)). For a full list of available results, see [](S07_Retrieving_and_drawing_results.md).

## Diagnosing volume results

Using the 3D Metrology module, you can create index, mask, and status images for use when diagnosing volume operation results. Use [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) to copy results into an image buffer to create:

- An index image in which a pixel's gray value is set to the index of the volume element that was used to compute the volume ([`M_VOLUME_ELEMENT_INDEX_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md)).
- A mask image whose pixels are non-zero for volume elements that were used for the volume computation, and zero otherwise ([`M_VOLUME_ELEMENT_MASK`](../../Reference/3dmet/M3dmetCopyResult.md)).
- A mask image whose pixels are non-zero for source points that contributed to the volume computation, and zero otherwise ([`M_VOLUME_SOURCE_POINTS_MASK`](../../Reference/3dmet/M3dmetCopyResult.md)).
- A status image whose pixels represent how the source volume elements contributed to the volume calculation ([`M_VOLUME_ELEMENT_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md)).
- A status image whose pixels represent how the source points contributed to the volume calculation ([`M_VOLUME_SOURCE_POINTS_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md)).

A volume element's index is its numbered position in the source container's mesh component or in the source depth map image buffer. For a container, the mesh component is one-dimensional, and indexing begins at 0. For a depth map, indexing begins at 0 for the upper left position and proceeds from left to right across all rows in the 2D pixel grid.

Each pixel of a status image created with [`M_VOLUME_ELEMENT_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md) can have one of up to 8 possible status values, as listed in the table below.

|   |   |   |   |
| --- | --- | --- | --- |
| Unused | Positive | Negative | Both |
| 0 | 1 (0b00000001) | 2 (0b00000010) | 3 (0b00000011) |
| Unused Spliced | Positive Spliced | Negative Spliced | Both Spliced |
| 4 (0b00000100) | 5 (0b00000101) | 6 (0b00000110) | 7 (0b00000111) |

Note that spliced volume elements are those that intersect the reference object (only for source point clouds). See the illustration below.

*[Image: 3dmet_spliced_volume_element.png]*

Whether the status of a spliced volume element is positive, negative, or both depends on the [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) and/or [`M_VOLUME_OUTPUT_MODE`](../../Reference/3dmet/M3dmetControl.md) control type settings, set using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md). A spliced volume element receives a status of both if one part of the spliced volume contributes positively and the other part contributes negatively (for example, when the specified volume mode is [`M_DIFFERENCE`](../../Reference/3dmet/M3dmetControl.md)).

When working with status images, it might be useful to map the status image through a LUT to display each volume element's (or source point's) status in different colors, which can help you visualize individual contributions to the volume operation. For example, you can map the status image of a depth map volume calculation through a LUT so that positive status values display in one color and negative status values display in a contrasting color.

You can also use [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) to copy volume results into a container or depth map image buffer. The resulting object can help when diagnosing certain volume results. You can copy results into a:

- Container to hold projected source points that were used to compute the volume ([`M_VOLUME_REFERENCE_CONTAINER`](../../Reference/3dmet/M3dmetCopyResult.md)). Note that the reference object must have been planar.
- Depth map in which pixel values will represent volume element depths ([`M_VOLUME_REFERENCE_DEPTH_MAP`](../../Reference/3dmet/M3dmetCopyResult.md)). Note that the source object must have been a depth map.

The _CalculateVolumeDiagnostic.cpp_ example demonstrates how to use the 3D Metrology module to calculate a volume and diagnose the result. To run/view this and other examples, use Aurora Imaging Example Launcher.
