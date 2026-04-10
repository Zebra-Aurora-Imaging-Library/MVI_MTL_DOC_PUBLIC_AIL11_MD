---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Correlation_stitching_registration
section: Retrieving_and_analyzing_correlation_stitching_registration_settings
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / CorrelationStitchingRegistration / Retrieving and analyzing correlation stitching registration settings"
---

# Retrieving and analyzing results

After having successfully registered your images using [`MregCalculate`](../../Reference/reg/MregCalculate.md), you can extract the required results from your result buffer using [`MregGetResult`](../../Reference/reg/MregGetResult.md).

## Possible results

Registration produces several types of results which provide information on the calculated positions for the images in the global pixel coordinate system and the success of the registration calculation. Results can be returned for:

- Whether the registration was successful, globally or for an individual image ([`M_RESULT`](../../Reference/reg/MregGetResult.md)).
- The score of the registration, globally or for an individual image ([`M_SCORE`](../../Reference/reg/MregGetResult.md)).
- The X- and Y-coordinates of the origin of each image's pixel coordinate system in the global/reference coordinate system ([`M_POSITION_X`](../../Reference/reg/MregGetResult.md) and [`M_POSITION_Y`](../../Reference/reg/MregGetResult.md)).
- The coordinates of the four corners of each image in the global/reference coordinate system ([`M_TRANSFORMED_...`](../../Reference/reg/MregGetResult.md)).
- The identifier of the internal buffer that stores the forward or reverse transformation matrix, used to transform a position in an image into its position in the global/reference coordinate system or vice versa ([`M_TRANSFORMATION_MATRIX_ID`](../../Reference/reg/MregGetResult.md), [`M_REVERSE_TRANSFORMATION_MATRIX_ID`](../../Reference/reg/MregGetResult.md)).
- The forward or reverse transformation matrix data, used to transform a position in an image into its position in the global/reference coordinate system or vice versa ([`M_TRANSFORMATION_MATRIX`](../../Reference/reg/MregGetResult.md), [`M_REVERSE_TRANSFORMATION_MATRIX`](../../Reference/reg/MregGetResult.md)).
- The width and height that the mosaic will have when it is composed ([`M_MOSAIC_SIZE_X`](../../Reference/reg/MregGetResult.md) and [`M_MOSAIC_SIZE_Y`](../../Reference/reg/MregGetResult.md)). This information is useful to determine the size of the destination image buffer needed to hold the mosaic.

You can retrieve general results for the entire registration calculation or specific results for each registration image. For a complete description of all possible results, refer to the description of [`MregGetResult`](../../Reference/reg/MregGetResult.md) in the Aurora Imaging Library Reference.

## Using the results

There are several ways that you can use the results of the registration. You can use them to create a mosaic, or to improve the resolution of the images using super-resolution. For more information on mosaicing, see [Mosaicing and super-resolution](S09_Mosaicing.md). Furthermore, you can use [`MregTransformCoordinate`](../../Reference/reg/MregTransformCoordinate.md) or [`MregTransformCoordinateList`](../../Reference/reg/MregTransformCoordinateList.md) to convert a pair or a list of coordinates between two of the following coordinate systems: the global pixel coordinate system, an image's pixel coordinate system, or a mosaic's coordinate system. In addition, you can pass the retrieved identifier of the buffer containing the forward or reverse transformation matrix to [`MimWarp`](../../Reference/im/MimWarp.md) and use it to warp other images.

## Drawing results

Using the [`MregDraw`](../../Reference/reg/MregDraw.md) function, you can draw boxes in a destination image buffer or 2D graphics list that outline the calculated positions (in the global pixel coordinate system) for the images. You can choose to draw in the display's overlay buffer. By drawing into the display's overlay buffer, you can annotate an image non-destructively (see [Annotating the displayed image non-destructively](../C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md)).

You can use a previously allocated 2D graphics context (see [Generating graphics](../C26_Generating_graphics/ChapterInformation.md)) to control the drawing color, or use the default 2D graphics context ([`M_DEFAULT`](../../Reference/reg/MregDraw.md)).

You can also draw a zoomed version of the boxes that outline the images' positions. To do so, use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md), [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md), [`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md), and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md). The relative origin values must be specified in pixels and represent the coordinates in the mosaic's coordinate system that will appear at the top-left corner of the destination buffer. The scale values specify the X- and Y-scaling factors used to fill the destination buffer.
