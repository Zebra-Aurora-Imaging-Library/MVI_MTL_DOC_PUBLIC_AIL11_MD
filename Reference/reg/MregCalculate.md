---
doctype: Reference
module: reg
function: MregCalculate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregCalculate"
---

# MregCalculate

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Perform the registration operation specified by the registration context on the input images.

## Syntax

```c
void MregCalculate(
    AIL_ID         ContextId,                 //in
    const AIL_ID * ImageOrContainerArrayPtr,  //in
    AIL_ID         RegResultOrImageId,        //out
    AIL_INT        NumImages,                 //in
    AIL_INT64      ControlFlag                //in
)
```

## Description

This function performs the registration operation specified by the registration context on the input images. It can perform a correlation-stitching registration operation, an extended depth of field registration operation, a depth-from-focus registration operation, a high dynamic range registration operation, or a photometric stereo registration operation.

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the context to use for the registration; this establishes the operation to perform. The context must have been allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md).

### `ImageOrContainerArrayPtr` *(in, *AIL_ID)*

Specifies the address of the array containing the buffer identifiers of the input images or specifies the address of the variable containing the identifier of the image buffer container. The image buffers must be 1- or 3-band buffers and must not have a region of interest (ROI) associated with them. Using images with an ROI will cause an error.

### `RegResultOrImageId` *(out, AIL_ID)*

Specifies the identifier of the registration result buffer or image buffer in which to write the results of the registration operation.

### `NumImages` *(in, AIL_INT)*

Specifies the size of the specified image buffer array, or specifies the number of image buffer components in the specified image buffer container.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the operation mode.

## Parameter Associations

### For specifying the operation mode

---

### `M_DEFAULT_EXTENDED_DEPTH_OF_FIELD_CONTEXT`

Same as [`EDoFContextId`](../../Reference/reg/MregCalculate.md). Specifies the default [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md) registration context with all the control types ([`MregControl`](../../Reference/reg/MregControl.md)) in the context set to [`M_DEFAULT`](../../Reference/reg/MregControl.md).

---

### `Correlation-stitching registration context identifier`

Specifies a valid correlation-stitching registration context identifier, previously allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_STITCHING`](../../Reference/reg/MregAlloc.md).  When this context is passed, a correlation-stitching registration operation is performed. The function calculates the optimal match in the overlapping region between each image and its reference image and then calculates the transformation needed to achieve this match. Once the transformation that maps each image into its reference image's pixel coordinate system is calculated, it is converted so that it maps the image into the global pixel coordinate system.  Typically for a correlation-stitching registration context, every input image is associated with a registration element that provides the rough location of the image in another input image (reference image) or in the global pixel coordinate system.

#### `M_DEFAULT`

Specifies the default correlation-stitching registration operation mode.

---

### `Depth-from-focus registration context identifier`

Specifies a valid depth-from-focus registration context identifier, previously allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_DEPTH_FROM_FOCUS`](../../Reference/reg/MregAlloc.md). When this context is passed, a depth-from-focus registration operation is performed. This operation generates an image that conveys depth information, from a set of two-dimensional images of the same scene taken at different focus distances. The operation calculates, for each pixel, in which image that pixel is found most in focus. It replaces the corresponding pixel entry with the index of the image. This output image is referred to as the index map.  The depth-from-focus operation requires that every input image is of the same scene, at different focus levels.

#### `M_DEFAULT`

Same as [`M_COMPUTE`](../../Reference/reg/MregCalculate.md).

#### `M_ACCUMULATE_AND_COMPUTE`

Specifies to process additional input images, and then calculate the index map using this new information and all of the information previously processed and stored in the result buffer. Each call to [`MregCalculate`](../../Reference/reg/MregCalculate.md) with this constant will accumulate new results and a new index map in the depth-from-focus result buffer. This operation will not empty the result buffer of previous entries. If an index map is already in the result buffer prior to [`MregCalculate`](../../Reference/reg/MregCalculate.md), it will be discarded and replaced with the new index map.  Note that when using this value with a non-empty result buffer, the settings of the specified context must match the settings of the context used to add the previous image information to the result buffer, otherwise an error occurs.  You can retrieve preprocessing results from a depth-from-focus result buffer using [`MregGetResult`](../../Reference/reg/MregGetResult.md), and access the depth-from-focus index map by drawing it using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_DEPTH_INDEX_MAP`](../../Reference/reg/MregDraw.md).

#### `M_COMPUTE`

Specifies to calculate the index map from the specified input images.  If passing a result buffer to [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md), [`MregCalculate`](../../Reference/reg/MregCalculate.md) will clear the result buffer of previous results, and then fill the buffer with the index map and new results.  You can retrieve preprocessing results from a depth-from-focus result buffer using [`MregGetResult`](../../Reference/reg/MregGetResult.md), and access the depth-from-focus index map by drawing it using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_DEPTH_INDEX_MAP`](../../Reference/reg/MregDraw.md).  If passing an image buffer to [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md), the function clears the image buffer, and writes the resulting index map in the image buffer; no other results are accessible.

| Value | Description |
| --- | --- |
| `Depth-from-focus registration result buffer identifier` | Specifies a valid depth-from-focus registration result buffer identifier, previously allocated using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md). |
| `Image buffer identifier` | Specifies the identifier of a valid image buffer, previously allocated on the required system using [`MbufAlloc...`](../../Reference/buf/MbufAlloc2d.md). The image buffer must have a type large enough to contain the maximum number of indices ([`NumImages`](../../Reference/reg/MregCalculate.md)). |

#### `M_RESET`

Specifies to clear the specified result buffer.

---

### `Extended depth of field registration context identifier`

Specifies a valid extended depth of field registration context identifier, previously allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md).  When using an [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md) registration context, this function performs an extended depth of field registration operation. It will preprocess the specified input images and then fuse the images to create an extended depth of field (EDoF) image. The results can be stored in a previously allocated result buffer or an image buffer.  Each input image must be of the same scene, taken under different focus distances; the operation combines the focused objects in all of the images to create an image with all the objects of interest in focus.  In the [`M_ACCUMULATE`](../../Reference/reg/MregCalculate.md) and [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md) modes, images must be preprocessed using the same registration context and settings (for example, same maximum radius of the circle of confusion).  You can retrieve results from an extended depth of field result buffer using [`MregGetResult`](../../Reference/reg/MregGetResult.md), and access the EDoF image by drawing it using [`MregDraw`](../../Reference/reg/MregDraw.md).

#### `M_DEFAULT`

Same as [`M_COMPUTE`](../../Reference/reg/MregCalculate.md).

#### `M_ACCUMULATE`

Specifies to preprocess the images and to add the information to the result buffer. This will not empty the result buffer prior to writing to it, nor calculate the extended depth of field (EDoF) image. To create the EDoF image after accumulating, you must call [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md).  Note that when using this value with a non-empty result buffer, the settings of the specified context must match the settings of the context used to add the previous image information to the result buffer.

#### `M_ACCUMULATE_AND_COMPUTE`

Specifies to preprocess the additional input images, and then compute the EDoF image using this new information and all of the information previously processed and stored in the result buffer. The EDoF image will only be calculated after the last image has been processed. This will not empty the result buffer of previous entries. If an EDoF image is already in the result buffer prior to [`MregCalculate`](../../Reference/reg/MregCalculate.md), it will be discarded and replaced with the new EDoF image.  Note that you don't have to pass additional input images. If no input images are specified, the operation calculates the EDoF image using only the preprocessed information in the result buffer.  Note that when using this value with a non-empty result buffer, the settings of the specified context must match the settings of the context used to add the previous image information to the result buffer.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to calculate the EDoF image using only the preprocessed information available in the result buffer. |
| `Array of image buffer IDs` | Specifies the address of an array of Aurora Imaging Library image buffer identifiers to use, besides also using the preprocessed information available in the result buffer. The input images must have the same size, type, and number of bands.  You can replace images in the array that you do not want to use in the calculations with **M_NULL**. This allows you to establish the minimum number of focus distances at which to grab the input images and still obtain a good EDoF image. The fewer the number of input images, the faster the operation. For example, you can perform the operation with 10 different focus distances, and then try performing the same operation with every second image set to **M_NULL** to see if the EDoF image is still acceptable. |
| `Container ID` | Specifies the address of the variable containing the identifier of the image buffer container to use, besides also using the preprocessed information available in the result buffer. The input images must have the same size, type, and number of bands. |

#### `M_COMPUTE`

Specifies to calculate the EDoF image immediately.  If passing a result buffer to [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md), [`MregCalculate`](../../Reference/reg/MregCalculate.md) will clear the result buffer of previous results, and then fill the result with the preprocessing information and the EDoF image.  If passing an image buffer to [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md), the function clears the image buffer of any previous image, and writes the resulting EDoF image in the image buffer; no other results are accessible.

| Value | Description |
| --- | --- |
| `Extended depth of field registration result buffer identifier` | Specifies a valid extended depth of field registration result buffer identifier, previously allocated using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md).  You can retrieve preprocessing results from an extended depth of field result buffer using [`MregGetResult`](../../Reference/reg/MregGetResult.md), and access the EDoF image by drawing it using [`MregDraw`](../../Reference/reg/MregDraw.md). |
| `Image buffer identifier` | Specifies a valid image buffer, previously allocated on the required system using [`MbufAlloc...`](../../Reference/buf/MbufAlloc2d.md). |

#### `M_RESET`

Specifies to clear the result buffer of all preprocessing information and the EDoF image, if applicable.

---

### `High dynamic range registration context identifier`

Specifies a valid high dynamic range registration context identifier, previously allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md).  When this context is passed, a high dynamic range registration operation is performed. This function merges two or more images of the same subject that were taken with different exposure times, and therefore have different levels of saturation. Shorter exposure times produce darker images in which details in dark areas might not appear, and that have no saturation in brighter areas. Similarly, longer exposure times produce brighter images in which details in dark areas will appear, but brighter areas might begin to saturate (going over the sensor's light accumulation capacity) and lose detail. When the images are fused together, the resulting HDR image will show details in both dark areas and bright areas.  When finished, the function applies tone mapping. The tone mapping is non-linear and can be tuned to better highlight a range of pixel intensities in the resulting images, without truncating other pixel intensities. You can control tone mapping settings using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_TONE_MAPPING_...`](../../Reference/reg/MregControl.md).  You can retrieve results from a high dynamic range result buffer using [`MregGetResult`](../../Reference/reg/MregGetResult.md), and access the HDR image by drawing it using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_HDR_IMAGE`](../../Reference/reg/MregDraw.md).

#### `M_DEFAULT`

Same as [`M_COMPUTE`](../../Reference/reg/MregCalculate.md).

#### `M_ACCUMULATE`

Specifies to preprocess the images and to add the information to the result buffer. This will not empty the result buffer prior to writing to it, nor calculate the high dynamic range image. To create the HDR image after accumulating, you must call [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md).  Note that when using this value with a non-empty result buffer, the settings of the specified context must match the settings of the context used to add the previous image information to the result buffer.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the array size. |

#### `M_ACCUMULATE_AND_COMPUTE`

Specifies to process additional input images, and then calculate the HDR image using this new information and all of the information previously processed and stored in the result buffer. The HDR image is calculated only after the last image has been processed. This will not empty the result buffer of previous entries. If an HDR image is already in the result buffer prior to calling [`MregCalculate`](../../Reference/reg/MregCalculate.md), it is discarded and replaced with the new HDR image.  Note that you don't have to pass additional input images. If no input images are specified, the operation calculates the HDR image using only the preprocessed information in the result buffer.  Note that when using this value with a non-empty result buffer, the settings of the specified context must match the settings of the context used to add the previous image information to the result buffer, otherwise an error occurs.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to calculate the HDR image using only the preprocessed information available in the result buffer. |
| `Array of image buffer IDs` | Specifies the address of an array of Aurora Imaging Library image buffer identifiers to use, besides also using the preprocessed information available in the result buffer. The input images must have the same size, type, and number of bands.  You can replace images in the array that you do not want to use in the calculations with **M_NULL**. This allows you to establish the minimum number of exposures at which to grab the input images and still obtain a good HDR image. The fewer the number of input images, the faster the operation. For example, you can perform the operation with 10 different exposures, and then try performing the same operation with every second image set to **M_NULL** to see if the HDR image is still acceptable. |
| `Container ID` | Specifies the address of the variable containing the identifier of the image buffer container to use. |
| `0` | Specifies no input images. |
| `Value >= 1` | Specifies the array size. |

#### `M_COMPUTE`

Specifies to complete the HDR processing with the input images provided in the call. No previously added images are used.  If passing a result buffer to [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md), [`MregCalculate`](../../Reference/reg/MregCalculate.md) will clear the result buffer of previous results, and then fill the result with the preprocessing information and the HDR image. If passing an image buffer to [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md), the function clears the image buffer of any previous image, and writes the resulting HDR image in the image buffer; no other results are accessible.

| Value | Description |
| --- | --- |
| `High dyamic range registration result buffer identifier` | Specifies a valid high dynamic range registration result buffer identifier, previously allocated using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md). |
| `Image buffer identifier` | Specifies the identifier of a valid image buffer to hold the computed HDR image; the image buffer must have been previously allocated on the required system using [`MbufAlloc...`](../../Reference/buf/MbufAlloc2d.md). |
| `Value >= 2` | Specifies the array size. |

#### `M_RESET`

Specifies to clear the specified result buffer.

---

### `Photometric stereo registration context identifier`

Specifies a valid photometric stereo registration context identifier, previously allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md).  When this context is passed, a photometric stereo registration operation is performed. This operation generates an image that reveals raised and recessed features, surface reflectance changes, and/or surface defects. To highlight such features, the operation uses multiple images of the same scene taken under different directional lighting to produce a single enhanced image. The photometric stereo operation uses all of the source images jointly to compute the surface normal in each neighborhood of pixels, which is used to produce an albedo image, for example, and/or other photometric stereo images, depending on the operation specified with [`MregControl`](../../Reference/reg/MregControl.md).  The photometric stereo operation requires that every input image is of the same scene, with different lighting source orientations. In addition, it assumes no camera settings are changed while grabbing the image. To perform this operation, the photometric stereo context must contain a registration element for each input image. Each registration element must specify the direction of the light used for its corresponding input image.

#### `M_COMPUTE`

Specifies to calculate the photometric stereo image(s), and store results in a result buffer or an image buffer.  Use [`MregControl`](../../Reference/reg/MregControl.md) to specify the type of photometric stereo computation.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of the image buffer in which to store results.  Specify the type of image to output using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_DRAW_WITH_NO_RESULT`](../../Reference/reg/MregControl.md). |
| `Photometric stereo registration result buffer identifier` | Specifies the identifier of the result buffer in which to store results. The identifier must be a valid photometric stereo registration result buffer identifier, previously allocated using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md).  If passing a result buffer to [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md), [`MregCalculate`](../../Reference/reg/MregCalculate.md) will clear the result buffer of previous results, and then fill the result buffer with the photometric stereo image(s) and/or other new results.  You can access the calculated images by drawing them using [`MregDraw`](../../Reference/reg/MregDraw.md), provided you have passed a registration result buffer using [`MregControl`](../../Reference/reg/MregControl.md). |
| `Value > 3` | Specifies the size. |

Specifies the address of an array or of the identifier of an image buffer container that holds the input images to use when performing the registration operation.

The input images must have the same size, type, and number of bands.

When using an image buffer array, you can replace images in the array that you do not want to use in the calculations with **M_NULL**. This allows you to establish the minimum number of focus distances at which to grab the input images and still obtain a good EDoF image. The fewer the number of input images, the faster the operation. For example, you can perform the operation with 10 different focus distances, and then try performing the same operation with every second image set to **M_NULL** to see if the EDoF image is still acceptable.

Specifies the address of an array or of the identifier of an image buffer container that holds the input images to use when performing the registration operation.

These images must not have a region of interest (ROI) associated with them. Using images with an ROI will cause an error.

Specifies the size of the array when using an image buffer array, or the number of image buffers in the container.

When using an image buffer array and the array contains fewer image buffer identifiers than the specified size, the outstanding elements of the array should be set to **M_NULL**.

Specifies the size of the array when using an image buffer array, or the number of image buffers in the container.
