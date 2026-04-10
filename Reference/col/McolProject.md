---
doctype: Reference
module: col
function: McolProject
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolProject"
---

# McolProject

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

> Perform a color projection.

## Syntax

```c
void McolProject(
    AIL_ID    Src1ImageId,         //in
    AIL_ID    Src2ImageOrArrayId,  //in
    AIL_ID    DestImageOrArrayId,  //out
    AIL_ID    DestMaskImageId,     //out
    AIL_INT64 Operation,           //in
    AIL_INT64 ControlFlag,         //in
    AIL_INT * StatusPtr            //out
)
```

## Description

This function performs a color projection (transformation) of the source image, or returns a color projection matrix, which you can then apply to transform other images, using [`MimConvert`](../../Reference/im/MimConvert.md). [`McolProject`](../../Reference/col/McolProject.md) allows you to separate a selected color from a rejected one, or to convert color images to grayscale with minimal loss of information. You can also use this function to perform analytical tasks, such as calculating an image's covariance matrix or the principal components of an image's color information.

The calculated projection results are written in the specified destination buffer ([`DestImageOrArrayId`](../../Reference/col/McolProject.md)). If the calculated projection results create an image, you can restrict the area to which results are written in the destination buffer using a mask image ([`DestMaskImageId`](../../Reference/col/McolProject.md)).

Color projection is an operation that requires data defined in a Cartesian coordinate system (for example, RGB or CIELAB). A projection with HSL data would provide incoherent results.

> **Note:** This function is optimized for images with a width (X-size) greater than seven pixels.

## Parameters

### `Src1ImageId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer on which to perform the projection transformation. This must be a 3-band 8- or 16-bit unsigned image. Source images are interpreted per band, independently of the Aurora Imaging Library buffer format.

### `Src2ImageOrArrayId` *(in, AIL_ID)*

Specifies the identifier of an image buffer or Aurora Imaging Library array buffer that controls how the source pixels are interpreted.

### `DestImageOrArrayId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer in which to write the color projection (transformation) results. The destination buffer can be either an image or an Aurora Imaging Library array buffer.

### `DestMaskImageId` *(out, AIL_ID)*

Specifies the identifier of the destination mask image. The mask image must be monochrome. You can only specify a mask if the destination buffer is an image. If you do not require a mask, set this parameter to `M_NULL`.

### `Operation` *(in, AIL_INT64)*

Specifies the color projection operation.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the dynamic range on which the resulting projection is mapped. This is only applicable for [`M_PRINCIPAL_COMPONENT_PROJECTION`](../../Reference/col/McolProject.md).

### `StatusPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the status of the color projection operation. If you do not require this information, set this parameter to `M_NULL`.

## Parameter Associations

### For implementing the color projection

---

### `M_COLOR_SEPARATION`

Removes a color from the source image. In this case, you must identify the background, selected, and rejected colors. The rejected color indicates the color to remove.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of a 3x3 Aurora Imaging Library array buffer, where each row defines, in the following order, the background, the selected, and the rejected colors, to use to compute the projection matrix. |
| `Image buffer identifier` | Specifies the identifier of a data identification image buffer. This image determines which areas of the source image ([`Src1ImageId`](../../Reference/col/McolProject.md)) have the background, selected, and rejected colors; these colors are then used to calculate the projection matrix. To identify the source image colors, you must set the corresponding pixels in the data identification image to one of the label values below.  | Label value | Definition | | --- | --- | | **M_BACKGROUND_LABEL** | Identifies a background-color pixel. | | **M_REJECTED_LABEL** | Identifies a rejected-color pixel. | | **M_SELECTED_LABEL** | Identifies a selected-color pixel. |  The calculation uses the average color value of all the pixels in the source image identified as background, selected, and rejected. You must assign at least one pixel in the data identification image to each of these values. Pixels in the source image that do not correspond to one of these values are ignored in the calculation. If the data identification image is smaller than the source image, the outlying source image pixels are ignored.  The data identification buffer must be a 1-band 8-bit unsigned image buffer and must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |
| `Array buffer identifier` | Specifies the identifier of a 4x3 Aurora Imaging Library array buffer, of type _float_, to store the resulting projection matrix.  You can use this matrix with [`MimConvert`](../../Reference/im/MimConvert.md) to separate colors in any 3-band color image. Be certain that the color distribution of all source images that use this matrix is similar, otherwise unpredictable results can occur. |
| `Image buffer identifier` | Specifies the identifier of a 3-band color image buffer, to store the transformed (color separated) source image.  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |
| `M_3_COLORS_COLLINEAR` | Specifies an unsatisfactory status, indicating that the selected, rejected, and background colors are collinear. |
| `M_NO_BACKGROUND_DEFINED` | Specifies an unsatisfactory status, indicating that no background color was defined with the [`Src2ImageOrArrayId`](../../Reference/col/McolProject.md) parameter. |
| `M_NO_REJECTED_DEFINED` | Specifies an unsatisfactory status, indicating that no rejected color was defined with the [`Src2ImageOrArrayId`](../../Reference/col/McolProject.md) parameter. |
| `M_NO_SELECTED_DEFINED` | Specifies an unsatisfactory status, indicating that no selected color was defined with the [`Src2ImageOrArrayId`](../../Reference/col/McolProject.md) parameter. |
| `M_REJECTED_EQUAL_BACKGROUND` | Specifies an unsatisfactory status, indicating that the rejected color equals the background color. |
| `M_REJECTED_EQUAL_SELECTED` | Specifies an unsatisfactory status, indicating that the rejected color equals the selected color. |
| `M_SELECTED_EQUAL_BACKGROUND` | Specifies an unsatisfactory status, indicating that the selected color equals the background color. In this case, although the operation was performed, Aurora Imaging Library internally used, as the selected color, a color that is perpendicular to the rejected color. |
| `M_SUCCESS` | Specifies a successful operation. |

---

### `M_COVARIANCE`

Calculates the covariance matrix of the source image's color information. This is a measure of the color variation.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to use the entire source image to compute the covariance matrix. |
| `Image buffer identifier` | Specifies the identifier of a data identification image buffer. This image determines which areas of the source image ([`Src1ImageId`](../../Reference/col/McolProject.md)) to use to compute the covariance matrix. To specify an area, you must set the corresponding pixels in the data identification image to **M_SOURCE_LABEL**.  You must assign at least one pixel in the data identification image to **M_SOURCE_LABEL**. Pixels in the source image that do not correspond to **M_SOURCE_LABEL** are ignored in the calculation. If the data identification image is smaller than the source image, the outlying source image pixels are ignored.  The data identification buffer must be a 1-band 8-bit unsigned image buffer and must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |
| `M_NO_SOURCE_DEFINED` | Specifies an unsatisfactory status, indicating that there was no pixel in the data identification image that was set to **M_SOURCE_LABEL**. |
| `M_SUCCESS` | Specifies a successful operation. |

---

### `M_PRINCIPAL_COMPONENT_PROJECTION`

Performs a principal component projection of the source image. This converts each color pixel to a grayscale value with a point-to-point projection of the source image, onto its first principal component, using a grayscale LUT. The projection is a mathematically optimal color-to-grayscale transformation, which uses the distribution of the source image's color data to calculate the best grayscale conversion possible, minimizing the loss of information.  Aurora Imaging Library performs a principal component analysis (PCA) to calculate the color image's principal components. The strongest is the first principal component. If no principal component is the strongest, one is arbitrarily considered the first principal component.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to use the entire source image to perform the principal component projection. |
| `Image buffer identifier` | Specifies the identifier of a data identification image buffer. This image determines which areas of the source image ([`Src1ImageId`](../../Reference/col/McolProject.md)) to use to calculate the first principal component, which is used to perform the principal component projection. To specify an area, you must set the corresponding pixels in the data identification image to **M_SOURCE_LABEL**.  If there is ambiguity between which colors should be designated as light and dark, set some pixels in the data identification image to **M_BRIGHT_LABEL** and **M_DARK_LABEL**. **M_BRIGHT_LABEL** identifies source pixels that represent the color to project on the brightest side of the grayscale palette, while **M_DARK_LABEL** identifies source pixels that represent the color to project on the darkest side. These labels are optional and pixels designated as bright or dark are not used in the principal component projection for anything other than determining which colors are light and dark. Therefore, use as few pixels as necessary for this purpose. Aurora Imaging Library uses the mean color of the bright or dark pixels if you set multiple pixels to bright or dark.  You must assign at least one pixel in the data identification image to **M_SOURCE_LABEL**. Pixels in the source image that do not correspond to **M_SOURCE_LABEL** are ignored in the calculation. If the data identification image is smaller than the source image, the outlying source image pixels are ignored.  The data identification image must be a 1-band 8-bit unsigned image buffer and must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |
| `Array buffer identifier` | Specifies the identifier of a 4x1 Aurora Imaging Library array buffer, of type _float_, to store the resulting projection matrix.  You can use this matrix with [`MimConvert`](../../Reference/im/MimConvert.md) to perform a color-to-grayscale transformation of any 3-band color image. Be certain that the color distribution of all source images that use this matrix is similar, otherwise unpredictable results can occur. |
| `Image buffer identifier` | Specifies the identifier of a 1-band image buffer, to store the transformed source image.  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |
| `M_DEFAULT` | Performs the projection according to the dynamic range of all the pixels in the source image. |
| `M_MASK_CONTRAST_ENHANCEMENT` | Performs the projection according to the dynamic range of the pixels identified with the data identification image. The result is similar to increasing the contrast. When using [`M_MASK_CONTRAST_ENHANCEMENT`](../../Reference/col/McolProject.md), Aurora Imaging Library saturates the projected unidentified source image colors that are outside the dynamic range of the identified source image pixels' color; that is, Aurora Imaging Library maps those colors to either the minimum or maximum allowable grayscale value. In this case, the projection result can contain sections that have lost a certain degree of contrast information. |
| `M_NO_SOURCE_DEFINED` | Specifies an unsatisfactory status, indicating that there was no pixel in the data identification image that was set to **M_SOURCE_LABEL**. |
| `M_SUCCESS` | Specifies a successful operation. |
| `M_UNSTABLE_POLARITY` | Specifies an unsatisfactory status, indicating that the polarity of the projection is unstable. In this case, although the operation was performed, it was not possible to confidently determine, in a consistent and repeatable way, which pixels should be projected to the brightest/darkest side of the grayscale palette. |

---

### `M_PRINCIPAL_COMPONENTS`

Calculates the principal components of the source image's color information. The principal components correspond to the three eigenvectors of the image's covariance matrix. With [`M_PRINCIPAL_COMPONENTS`](../../Reference/col/McolProject.md), you can also retrieve each eigenvector's eigenvalue and the source image's average color.  To calculate the principal components, Aurora Imaging Library performs a principal component analysis (PCA).

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to use the entire source image to calculate the principal components. |
| `Image buffer identifier` | Specifies the identifier of a data identification image buffer. This image determines which areas of the source image ([`Src1ImageId`](../../Reference/col/McolProject.md)) to use to calculate the principal components. To specify an area, you must set the appropriate pixels in the data identification image to **M_SOURCE_LABEL**.  You must assign at least one pixel in the data identification image to **M_SOURCE_LABEL**. Pixels in the source image that do not correspond to **M_SOURCE_LABEL** are ignored in the calculation. If the data identification image is smaller than the source image, the outlying source image pixels are ignored.  The data identification image must be a 1-band 8-bit unsigned image buffer and must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |
| `Array buffer identifier` | Specifies the identifier of a 3x3, 4x3, or 5x3 Aurora Imaging Library array buffer, of type _float_, to store the resulting data. The data returned depends on the array's dimensions:  - 3x3.   The three eigenvectors are returned columnwise in decreasing order of strength. - 4x3.   In addition to the 3x3 array results, each eigenvectors's eigenvalue is returned in the fourth column. The eigenvalue corresponds to the eigenvector's strength. - 5x3.   In addition to the 4x3 array results, the mean color of the source image areas (**M_SOURCE_LABEL**) is returned in the fifth column, band-by-band: _SourceMeanValueBand0_, _SourceMeanValueBand1_, and _SourceMeanValueBand2_. |
| `M_NO_SOURCE_DEFINED` | Specifies an unsatisfactory status, indicating that there was no pixel in the data identification image that was set to **M_SOURCE_LABEL**. |
| `M_SUCCESS` | Specifies a successful operation. |
| `M_UNSTABLE_POLARITY` | Specifies an unsatisfactory status, indicating that the direction of the first principal component is unstable. In this case, although the operation was performed, it was not possible to align the first principal component, in a consistent and repeatable way, with the gray axis (from black to white). |
| `M_UNSTABLE_PRINCIPAL_COMPONENT_2` | Specifies an unsatisfactory status, indicating that the direction of the third principal component (least important) is unstable. Although the operation was performed, this status result typically means that there were small variances along the color component's direction. |
| `M_UNSTABLE_PRINCIPAL_COMPONENTS_12` | Specifies an unsatisfactory status, indicating that the direction of the second and third principal components are unstable. Although the operation was performed, this status result typically means that there were small variances along the color component's direction. |
| `M_UNSTABLE_PRINCIPAL_COMPONENTS_012` | Specifies an unsatisfactory status, indicating that the direction of all principal components are unstable. |

If [`DestImageOrArrayId`](../../Reference/col/McolProject.md) specifies an image buffer, it is a copy of the source image. If [`DestImageOrArrayId`](../../Reference/col/McolProject.md) specifies an array buffer, it is filled with the identity matrix such that, using this array with [`MimConvert`](../../Reference/im/MimConvert.md) copies the source image into the destination image.

The resulting matrix ([`DestImageOrArrayId`](../../Reference/col/McolProject.md)) is filled with zeros.
