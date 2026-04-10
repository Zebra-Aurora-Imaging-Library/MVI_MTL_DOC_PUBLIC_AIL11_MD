---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Registration
section: Photometric_Stereo
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / registration / Photometric Stereo"
---

# Surface feature enhancement and defect detection using photometric stereo registration

With a given lighting orientation, all the details on an object's surface in an image might not be detectable. To detect these characteristics, you can perform a photometric stereo registration operation using the Aurora Imaging Library Registration module. This operation takes multiple images of the same scene taken under different lighting orientations. It is assumed that the camera does not move and no other camera settings are changed while grabbing the series of images. The images are used together to create a single composite image. This single image reveals raised and recessed features and surface reflectance changes that are not apparent with an image taken with only one light source. Photometric stereo registration operations use material reflectance properties and the surface curvature of objects when calculating the resulting enhanced image. Operations available include computing the albedo image, Gaussian curvature image, mean curvature image, local shape image, local contrast image, and local texture image.

The following animation illustrates the process of photometric stereo registration, resulting in an enhanced image.

*[Image: Photometrics_Overview_Three]*

You can use four or more lights in the photometric stereo setup. Note that when setting up for a local shape image operation, while each input image uses one light source, your lights must be set up in opposite pairs. That is, for each light in the setup, there should be another light positioned 180 degrees opposite.

You can also perform photometric stereo registration on a moving object (for example, on a conveyor belt). In this case, the source images must be aligned before the photometric stereo registration can take place. For more information, see [Photometric stereo registration of a moving object](S05_Photometric_Stereo.md).

## Steps to performing a photometric stereo registration

The following steps provide a basic methodology for performing a photometric stereo operation using multiple images:

1. Allocate a photometric stereo registration context, using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md).
2. Optionally, allocate a photometric stereo registration result buffer to hold the results of the registration operation, using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md). If you are only interested in one of the resulting photometric stereo images, you don't need a result buffer.
3. If necessary, allocate an image buffer to hold the resulting image (alternatively, the image can be stored in the result buffer). The image buffer should be of the same size, type, and number of bands as the input images.
4. Set the type of photometric stereo image(s) to calculate, using [`MregControl`](../../Reference/reg/MregControl.md), depending on whether a result buffer or image buffer will hold the resulting photometric stereo image(s):
   - For a result buffer, you can enable one or more types of photometric stereo images for calculation using their dedicated control types. For example, enable a local shape operation using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_LOCAL_SHAPE`](../../Reference/reg/MregControl.md) and [`M_ENABLE`](../../Reference/reg/MregControl.md).
   - For an image buffer, specify the photometric stereo image(s) to calculate, using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_DRAW_WITH_NO_RESULT`](../../Reference/reg/MregControl.md) and the appropriate control value (for example, [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregDraw.md) for a local shape image).
5. Specify the position of the light used for each input image using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_LIGHT_VECTOR_COMPONENT_1`](../../Reference/reg/MregControl.md), [`M_LIGHT_VECTOR_COMPONENT_2`](../../Reference/reg/MregControl.md), and [`M_LIGHT_VECTOR_COMPONENT_3`](../../Reference/reg/MregControl.md).
   You must specify your lights in a counter-clockwise order, starting at the positive X-axis. To illustrate, if you have four lights spaced evenly around a scene, and your first light is placed in line with the X-axis (zero degrees), lights 2, 3, and 4 are placed at 90, 180, and 270 degrees, respectively, and must be specified in the same order.
6. Specify any additional control settings (for example, [`M_SHAPE_SMOOTHNESS`](../../Reference/reg/MregControl.md) for a local shape image) using [`MregControl`](../../Reference/reg/MregControl.md).
7. If necessary, specify [remap factor](S05_Photometric_Stereo.md) settings. The remap factor applies when writing results to a result buffer for a local shape, Gaussian curvature, or mean curvature operation.
8. Set up an array with the identifiers of image buffers that hold the input images. All the images should be of the same scene, taken with all the same camera settings except lighting, and in buffers of the same size, type, and number of bands.
9. Perform the photometric stereo operation, using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_COMPUTE`](../../Reference/reg/MregCalculate.md), the input image array, and either a destination image or a result buffer.
10. If necessary, retrieve the required results from the result buffer using [`MregGetResult`](../../Reference/reg/MregGetResult.md).
11. If you passed a result buffer to [`MregCalculate`](../../Reference/reg/MregCalculate.md), you can draw the photometric stereo image(s) from the result buffer into an image buffer using [`MregDraw`](../../Reference/reg/MregDraw.md). To draw any resulting image, you must have previously enabled the calculation of that image using [`MregControl`](../../Reference/reg/MregControl.md) as discussed above. Note that you do not have to enable an albedo image; it is always available to draw from the result buffer after [`MregCalculate`](../../Reference/reg/MregCalculate.md) is called. Drawable images include but are not limited to a local shape image ([`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregDraw.md)) and a Gaussian curvature image ([`M_DRAW_GAUSSIAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md)). Use [`MregGetResult`](../../Reference/reg/MregGetResult.md) to establish the size and type of the image buffer required to draw each of the resulting images.
12. If necessary, save your registration context, using [`MregSave`](../../Reference/reg/MregSave.md) or [`MregStream`](../../Reference/reg/MregStream.md).
13. Free your allocated registration objects, using [`MregFree`](../../Reference/reg/MregFree.md), unless [`M_UNIQUE_ID`](../../Reference/reg/MregAlloc.md) was specified during allocation.

## Customizing your photometric stereo context

To perform a photometric stereo registration operation, you must first specify the number of registration elements using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_NUMBER_OF_REGISTRATION_ELEMENTS`](../../Reference/reg/MregControl.md). Each registration element holds information related to the position and relative intensity of the light source, for its associated image. Each image in the image array passed to [`MregCalculate`](../../Reference/reg/MregCalculate.md) is associated with the registration element of the same array index. The number of registration elements also sets the maximum number of light sources.

You can specify the position of the light source using the spherical coordinate system (default) or the Cartesian coordinate system, using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregControl.md). The spherical coordinate system defines orientations with 3 values: the polar (zenith) angle (angle between a light and the camera), the azimuth angle (angle of the light vector projected onto the XY-plane, measured from the positive X-axis), and the relative light intensity value. The Cartesian coordinate system specifies that the light vector is defined with Cartesian coordinates (X,Y, and Z), with the axes oriented to follow the right-hand rule (see [Coordinate systems](../C28_Calibration/S04_Coordinate_systems.md)). Note that, when setting the coordinates for either system, the origin is located at the center of the scene, directly below the camera (assuming the camera is perpendicular to the scene).

*[Image: Photometric_Azimuth_Polar.png]*

Typically, spherical coordinates are used to specify lighting positions. Use [`MregControl`](../../Reference/reg/MregControl.md) to set the vector components for each light source with [`M_LIGHT_VECTOR_COMPONENT_1`](../../Reference/reg/MregControl.md) for the polar (zenith) angle, [`M_LIGHT_VECTOR_COMPONENT_2`](../../Reference/reg/MregControl.md) for the azimuth angle, and [`M_LIGHT_VECTOR_COMPONENT_3`](../../Reference/reg/MregControl.md) for the relative light intensity, which is typically set to 1.0.

If relative light intensity is set to 1.0 for all light sources, each light provides the same intensity to the setup, which is the ideal arrangement. If equal light intensities are not possible, set the strongest light to 1.0 and set the other light intensity values relative to this.

The primary axis, used to specify the coordinates of the location of the light sources, is the vector that goes from the camera to the image. It is typical for the camera to be perpendicular to the object's surface for this application.

## Remap factor

For consistent results when performing a photometric stereo operation based on shape (such as a local shape, Gaussian curvature, or mean curvature operation), you can set a remap factor. Shape-based operations are calculated using normal vectors whose magnitudes can vary widely depending on image content. Internal calculations are done in floating-point. However, to generate the photometric stereo image with the same depth as the source images, the calculated values must be mapped or scaled to integer intensity values. By default, a remap factor is automatically calculated ([`M_DRAW_REMAP_FACTOR_MODE`](../../Reference/reg/MregControl.md) set to [`M_DEFAULT`](../../Reference/reg/MregControl.md) or [`M_AUTO`](../../Reference/reg/MregControl.md)), which remaps values using the full range of values in the current image. However, with some objects and lighting, it is possible for some (but not all) source images to have peak values that are much bigger than the others in the set (for example, a random, very sharp reflection), which causes that image to use a very different remap factor, possibly diminishing important details in key parts of the image. To avoid the effect of occasional sharp transitions, you can set a constant factor to use for the remapping ([`M_DRAW_REMAP_FACTOR_MODE`](../../Reference/reg/MregControl.md) set to [`M_USER_DEFINED`](../../Reference/reg/MregControl.md)). This is useful when you are performing several shape-based operations and want to ensure that the stored results are directly comparable.

Before setting the remap factor ([`M_DRAW_REMAP_FACTOR_VALUE`](../../Reference/reg/MregControl.md)), it is recommended to find the reported automatic mode factor (using [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_RANGE_FACTOR_LOCAL_SHAPE`](../../Reference/reg/MregGetResult.md), [`M_RANGE_FACTOR_GAUSSIAN_CURVATURE`](../../Reference/reg/MregGetResult.md) or [`M_RANGE_FACTOR_MEAN_CURVATURE`](../../Reference/reg/MregGetResult.md)) for some typical images. If the resulting output is satisfactory, set [`M_DRAW_REMAP_FACTOR_VALUE`](../../Reference/reg/MregControl.md) to the automatically calculated value. If not, you can try setting the remap factor to a higher value; this increases image contrast and helps to bring out smaller details in the photometric stereo image.

> **Note:** Note that you can only set the remap factor when writing results to a result buffer.

## Performing the photometric stereo registration operation and retrieving results

To perform the photometric stereo registration operation, call [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_COMPUTE`](../../Reference/reg/MregCalculate.md), and pass the array of input images required to generate the photometric stereo image(s) and either a destination image buffer or a photometric stereo result buffer. Subsequent calls to [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_COMPUTE`](../../Reference/reg/MregCalculate.md) will clear the photometric stereo registration result buffer before storing new results if a result buffer is specified.

You can specify the photometric stereo image(s) to compute using [`MregControl`](../../Reference/reg/MregControl.md). These images are: local shape image, albedo image (the default setting), Gaussian curvature image, mean curvature image, local contrast image, and texture image. Each computed photometric stereo image highlights different features or surface properties of the source scene. The following are descriptions of each operation. The related images show a source image on the left and the photometric stereo result (calculated from multiple source images) on the right.

- A local shape image operation ([`M_LOCAL_SHAPE`](../../Reference/reg/MregControl.md)) captures raised and recessed features on a surface. When there are 4 input images and the azimuth angles of their light sources are 0, 90, 180, and 270, respectively, you can use [`M_IMPROVED_LOCAL_SHAPE`](../../Reference/reg/MregControl.md), which computes an improved version of the local shape image. Note that [`M_LOCAL_SHAPE`](../../Reference/reg/MregControl.md) must be enabled to use [`M_IMPROVED_LOCAL_SHAPE`](../../Reference/reg/MregControl.md).
  The operation is fast with minimal noise in the resulting image. Embossed text often shows up well, which can facilitate Aurora Imaging Library SureDotOCR and String Reader operations. Use [`M_SHAPE_SMOOTHNESS`](../../Reference/reg/MregControl.md) to control shape smoothness in the photometric stereo result.
  *[Image: Photometric_LocShape_Compare.png]*
  Note how the raised and recessed surfaces on the object are revealed with the local shape operation.
  > **Note:** If opposite-paired lighting is not available for a local shape operation, a mean curvature operation can give similar, and sometimes better, results.
- An albedo image operation ([`M_DRAW_ALBEDO_IMAGE`](../../Reference/reg/MregControl.md)) highlights changes in surface reflectance, and is useful for finding a defect that reveals an underlying material that reflects differently than the surface material. Any change in surface reflectance, such as from shiny to dull, should be apparent in an albedo image result.
  *[Image: Photometric_Albedo_Compare.png]*
  Note the scratch revealed by the albedo image operation.
- A Gaussian curvature image operation ([`M_GAUSSIAN_CURVATURE`](../../Reference/reg/MregControl.md)) reveals sharp curvature changes, such as the corner of an object or a puncture in an otherwise flat surface. This operation will not pick up fine curvature detail. Also, if a surface contains flat printed areas, such as text or a logo, [`M_GAUSSIAN_CURVATURE`](../../Reference/reg/MregControl.md) ignores those differences, since a change in curvature is what matters. Use [`M_SHAPE_SMOOTHNESS`](../../Reference/reg/MregControl.md) to control shape smoothness in the photometric stereo result.
  *[Image: Photometric_GaussCurv_Compare.png]*
  Note how the printed text no longer obscures the defect, which is a large change in the surface curvature. A Gaussian curvature operation will not detect subtle surface curvature differences, such as the paper texture in the source image.
- The mean curvature image operation ([`M_MEAN_CURVATURE`](../../Reference/reg/MregControl.md)) can reveal small (local) curvature changes, because the curvature is calculated at every pixel, resulting in more local information. Fine scratches or very small features on a surface are revealed. Use [`M_SHAPE_SMOOTHNESS`](../../Reference/reg/MregControl.md) to control shape smoothness in the photometric stereo result.
  *[Image: Photometric_MeanCurv_Compare.png]*
  The operation reveals the defect while also showing finer surface features (the paper texture above). As with the Gaussian curvature operation, a mean curvature result is not sensitive to printed text.
  Note that the above result image was obtained using opposite-paired lighting, which is not required for the mean curvature operation, but is required for local shape results. Therefore, the mean curvature can substitute for local shape when opposite-paired lighting is not achievable.
- The local contrast image operation ([`M_LOCAL_CONTRAST`](../../Reference/reg/MregControl.md)) reveals surface height variations by increasing the contrast between highlights and shadows. A local contrast operation can remove printed text on flat surface areas in the resulting enhanced image, provided source image illumination is even (equal relative intensities across the light sources). This operation performs morphological operations using [`M_OBJECT_SIZE`](../../Reference/reg/MregControl.md) to control the number of iterations to perform. The number of iterations to perform is related to the feature size of the object(s) in the scene. For larger features, more iterations might be needed. The default is 1 iteration, but it is recommended to experiment to find the best setting for your needs.
  *[Image: Photometric_Local_Contrast_Compare.png]*
  Using the same source images as the Gaussian curvature operation, a local contrast operation reveals the defect and removes or reduces the printed text, while still showing surface texture.
- A texture image operation ([`M_TEXTURE_IMAGE`](../../Reference/reg/MregControl.md)) will show printing on a surface, since surface luminance is targeted. This operation can remove spectral reflections, such as those from plastic wrapping. For example, the printed address on a magazine wrapped in plastic becomes readable because the spectral reflections, which otherwise obscure the printing, are removed.
  *[Image: Photometric_Texture_Compare.png]*
  The texture image operation has removed spectral reflections, rendering the printed text and bar code readable in the photometric stereo result.

If [`MregCalculate`](../../Reference/reg/MregCalculate.md) used an image buffer as the destination, you can access the resulting image immediately after the registration operation. If [`MregCalculate`](../../Reference/reg/MregCalculate.md) used a result buffer as the destination, the result buffer holds the photometric stereo image(s) along with other results. You can draw the photometric stereo image(s) and/or other results into an image buffer using [`MregDraw`](../../Reference/reg/MregDraw.md). Use [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_STATUS`](../../Reference/reg/MregGetResult.md) to check if the photometric stereo image(s) is/are computed.

## Example of photometric stereo on non-moving objects

The animations below illustrate what happens in the subsequent example: how surface details can be extracted with the photometric stereo registration operation. The first animation shows an [`M_LOCAL_SHAPE`](../../Reference/reg/MregControl.md) operation, revealing the raised and recessed features of the source object. The subsequent animation shows an [`M_DRAW_ALBEDO_IMAGE`](../../Reference/reg/MregControl.md) operation that detects flaws on a leather surface.

*[Image: Photometric_Enhancement_Userguide]*

*[Image: Photometric_Userguide]*

The example _PhotometricStereo.cpp_ demonstrates several photometric stereo use cases. To run this and other examples, use Aurora Imaging Example Launcher.

## Photometric stereo registration of a moving object

Typically, the scene or object undergoing photometric stereo registration is stationary. However, photometric stereo registration is possible with images taken from a moving object (for example, on a conveyor).

In the case of a moving object, the source images must be aligned before you can perform the photometric stereo registration operation. To help calculate the object's displacement, take a start and an end image with all lights on. These two images serve as anchors for locating the object in the intermediate images, which are the photometric stereo source images taken with directional lighting.

Once you have acquired the start, end, and photometric stereo source images, apply the following:

1. Use the Pattern Matching or Model Finder module to define a model from the start image.
2. Find the model in the end image.
3. Using the model's location in the start and end images, calculate the model's displacement per photometric stereo image. Note that the object is assumed to have moved at a constant speed and in a straight line.
4. Translate all the photometric stereo source images such that the model is positioned in the same location for each image.
5. Perform the photometric stereo calculation.

The key step for a successful photometric stereo operation on a moving object is source image alignment. Using the above method, the defined model must be visible in the start and end images. In addition, the object's displacement must be small, relative to the lighting setup, so that distinct directional lighting is maintained between each image. A small displacement also minimizes perspective distortion and parallax errors. Your application could require a different methodology (for example, to account for source object rotation).

For an illustration of photometric stereo registration on a moving object, see the [PhotometricStereoWithMotion.cpp](../../Examples/PhotometricStereoWithMotion.cpp.md) example in Aurora Imaging Example Launcher.
