---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Defining_and_adding_models_to_your_model_finder_context
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Defining and adding models to your model finder context"
---

# Defining and adding models to your Model Finder context

Once you have allocated a Model Finder context using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md), you can begin to add models to your Model Finder context.

There are four different kinds of models that can be added to a Model Finder context:

- Model from an image.
- Synthetic model.
- Model from an Edge Finder or Model Finder result buffer.
- Model created from two other models.

In general, all of these models can be mixed in the same Model Finder context, using [`MmodDefine`](../../Reference/mod/MmodDefine.md) or [`MmodDefineFromFile`](../../Reference/mod/MmodDefineFromFile.md). The [`MmodDefine`](../../Reference/mod/MmodDefine.md) function can add image-type models, result-type models, and predefined-shape synthetic models; the [`MmodDefineFromFile`](../../Reference/mod/MmodDefineFromFile.md) function can add CAD-file synthetic models. If adding a model to a shape-specific Model Finder context, you can only add one model to it and it must be a synthetic model of the same type.

## Image-type models

You can add models defined from an image to a Model Finder context manually or automatically, using [`MmodDefine`](../../Reference/mod/MmodDefine.md) with [`M_IMAGE`](../../Reference/mod/MmodDefine.md) or [`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md), respectively. These types of models are referred to as **image-type** models.

The specified model source image must be a 1-band, 8-bit unsigned image.

When defining an image-type model manually, you must specify the region in the image from which to define the model. The minimum size of an image-type model is limited to 16.0 x 16.0 pixels, while the maximum size of 4096.0 x 4096.0 pixels is permitted. Note that image-type models are the only model type with a maximum permitted size of 4096.0 x 4096.0; all others have a maximum permitted size of 1024.0 x 1024.0.

*[Image: OffXOffY.png]*

To extract an image-type model from a rotated region of the model source image, you must specify a source image that is associated with a rotated rectangular region of interest (ROI). To define the ROI, add a rectangle graphic at the required angle to a 2D graphics list, using [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md). To set that rectangle graphic as the ROI of the model source image, you can call [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with either [`M_NO_RASTERIZE`](../../Reference/buf/MbufSetRegion.md) or [`M_RASTERIZE`](../../Reference/buf/MbufSetRegion.md) (specify [`M_FILL_REGION`](../../Reference/buf/MbufSetRegion.md) in either case), producing an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI or an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, respectively.

When defining a model automatically, Aurora Imaging Library searches the model source image for unique geometric features. When the most suitable geometric features are found, the function automatically defines a model from those features. To be useful, the model source image should be a typical target image; otherwise, the selected area for the model might never be present in the target. Defining a model automatically is useful, for example, when you want to perform whole image alignment, for which the displacement of a unique model from its original location specifies the displacement of the image. Aurora Imaging Library can automatically define a model based on specified settings or on default settings. The former is useful if you know, for example, that the selected model needs to be non-ambiguous only in a small range of angles; its ambiguity needs not be checked for the entire default range. Note that Aurora Imaging Library searches for the unique model far enough from the image borders so that if the target image is shifted from the model source image, the model can still be found; you specify the maximum displacement that can occur between the position of the model in the source image and the position of the model when found in the target image.

To define a model based on specified settings:

1. Define an empty image-type model, using [`MmodDefine`](../../Reference/mod/MmodDefine.md) with [`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md) set to [`M_NULL`](../../Reference/mod/MmodDefine.md).
2. Set the model's settings, using [`MmodControl`](../../Reference/mod/MmodControl.md) with the index of the empty model.
3. Define a unique geometric model from the model source image using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md) and the identifier of the source image. [`MmodControl`](../../Reference/mod/MmodControl.md) searches for a model with unique geometric features in the model source image that takes into account the settings previously specified using [`MmodControl`](../../Reference/mod/MmodControl.md).

To define a model based on default settings, use [`MmodDefine`](../../Reference/mod/MmodDefine.md) with [`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md) and the identifier of the source image.

### Extracting edges

When you call [`MmodDefine`](../../Reference/mod/MmodDefine.md), it stores the specified region of the model source image with the model. This image is referred to as the model image. The model source image is then no longer needed.

It is actually when you call [`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md) that edges are extracted from the model image. Specifically, [`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md) extracts the contour edges from the model image. For more information on contour edges, see [Extracting the edges](../C10_Edge_Finder/S04_Extracting_the_edges.md).

The edge extraction process involves a denoising operation to even out rough edges and reduce noise. You can control the degree of smoothness (strength) of the denoising operation used for all models in the context, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SMOOTHNESS`](../../Reference/mod/MmodControl.md). The range of this control type varies from 0.0 to 100.0; a value of 100.0 results in a strong noise reduction effect, while a value of 0.0 has almost no noise reduction effect. The default setting is 50.0.

*[Image: M_SMOOTHNESS.png]*

You can also control the detail level of the edge extraction process for all models in the context, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_DETAIL_LEVEL`](../../Reference/mod/MmodControl.md). Basically, this controls how much detail is extracted from the model image of each model in the context. The default setting ([`M_MEDIUM`](../../Reference/mod/MmodControl.md)) offers a robust detection of edges from images with contrast variation, noise, and non-uniform illumination. Nevertheless, in cases where objects of interest have a very low contrast compared to high contrast areas in the image, some low contrast edges can be missed.

The following examples show the use of the [`M_DETAIL_LEVEL`](../../Reference/mod/MmodControl.md) control type.

*[Image: M_DETAIL_LEVEL.png]*

If your images contain low-contrast and high-contrast objects, and your object of interest is low-contrast, a detail level setting of [`M_HIGH`](../../Reference/mod/MmodControl.md) should be used to ensure the detection of the low-contrast edges in the image. The [`M_VERY_HIGH`](../../Reference/mod/MmodControl.md) setting performs an exhaustive edge extraction, including very low contrast edges. However, it should be noted that this setting is very sensitive to noise.

Generally, the default settings for [`M_SMOOTHNESS`](../../Reference/mod/MmodControl.md) and [`M_DETAIL_LEVEL`](../../Reference/mod/MmodControl.md) are sufficient for the majority of images; you should adjust these settings only when dealing with very noisy images, extremely low or multi-contrasted images, or images with very thin, refined features. In such cases, it is recommended that you experiment with different settings to achieve the necessary level of accuracy and speed required by your application.

Note that the settings for [`M_SMOOTHNESS`](../../Reference/mod/MmodControl.md) and [`M_DETAIL_LEVEL`](../../Reference/mod/MmodControl.md) are also applied to the search target when it is an image.

## Synthetic models

Synthetic models are models that either have a predefined shape, or have been defined from a CAD DXF file. Synthetic models allow you to search the target for shapes.

### Predefined-shape models

You can use [`MmodDefine`](../../Reference/mod/MmodDefine.md) to add predefined-shape models to the context. The model types that are available are circle, cross, diamond, ellipse, rectangle, ring, square, and triangle. These model types are described in more detail in the description of [`MmodDefine`](../../Reference/mod/MmodDefine.md).

Use the [`MmodDefine`](../../Reference/mod/MmodDefine.md) parameters to define the attributes of the predefined-shape model, from the foreground color of the model to how it is constructed. The definition of each [`MmodDefine`](../../Reference/mod/MmodDefine.md) parameter depends on the model. For example, for a circle model, [`MmodDefine`](../../Reference/mod/MmodDefine.md) would have a parameter that defines the radius of the circle. For an ellipse model, [`MmodDefine`](../../Reference/mod/MmodDefine.md) would have parameters that define the width and the height.

You can also round all the corners of a model that has corners (the cross, diamond, rectangle, square, and triangle models), except for one in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) context. Set the radius of the curvature for the model's corners using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_CORNER_RADIUS`](../../Reference/mod/MmodControl.md).

*[Image: DefineCornerRadius.png]*

Predefined-shape models use the center of the shape as the origin (0,0).

### Predefined-shape models in specialized contexts

Aurora Imaging Library has specialized shape-specific contexts to perform more robust searches for circles, ellipses, and rectangles. Using a specialized context, you can also search for segments. Even though these context types are dedicated to finding a particular shape, you must still add a model to the context, and it must be of the same type as the context. For these types of contexts, you can only add a single model to the context.

Use [`MmodDefine`](../../Reference/mod/MmodDefine.md) as described above to add the model to the specialized shape-specific context. You can only add a synthetic segment model ([`M_SEGMENT`](../../Reference/mod/MmodDefine.md)) to an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) context. This allows you to search for occurrences of segments on contours of objects.

*[Image: Segment_Contour.png]*

Note that if a line is found in the target, it is counted as two segment occurrences in opposite directions. To restrict it to a single occurrence, set [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_MIN_SEPARATION_ANGLE`](../../Reference/mod/MmodControl.md) to [`M_DISABLE`](../../Reference/mod/MmodControl.md). See [Angle for rectangles and segments](S12_Search_settings_specific_to_models_in_specialized_shape_contexts.md).

Searching for a predefined-shape model using a shape-specific context has the following primary advantages:

- For an [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md) or [`M_SEGMENT`](../../Reference/mod/MmodDefine.md) synthetic model defined in a shape-specific context ([`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md), respectively), you can specify to find occurrences that are not perfectly straight or are broken up. To specify the tolerance for deformation, use [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_DEVIATION_TOLERANCE`](../../Reference/mod/MmodControl.md). For more information on this control types, see [Deviation tolerance for rectangles and segments](S12_Search_settings_specific_to_models_in_specialized_shape_contexts.md).
- For an [`M_CIRCLE`](../../Reference/mod/MmodDefine.md) synthetic model defined in an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) context, you can find occurrences that have deviation of the radii along the circumference of the model. For instance, an occurrence of a bottle cap could be identified as a circular model with the right radial deviation tolerance. You can control the radial deviation tolerance using [`M_SAGITTA_TOLERANCE`](../../Reference/mod/MmodControl.md). For more information regarding using this type of search constraint, see [Radial deviation tolerance](S12_Search_settings_specific_to_models_in_specialized_shape_contexts.md).
- For an [`M_ELLIPSE`](../../Reference/mod/MmodDefine.md) synthetic model defined in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) context, you can find occurrences of the model that have varying aspect ratios (for example, due to the perspective). You should define the ellipse model ([`MmodDefine`](../../Reference/mod/MmodDefine.md) with [`M_ELLIPSE`](../../Reference/mod/MmodDefine.md)) with the same width and height as that of the occurrence if the image had no distortion. If the image does have distortion that affects the occurrence's aspect ratio, you can use [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_MODEL_ASPECT_RATIO`](../../Reference/mod/MmodControl.md), [`M_MODEL_ASPECT_RATIO_MAX_FACTOR`](../../Reference/mod/MmodControl.md), and [`M_MODEL_ASPECT_RATIO_MIN_FACTOR`](../../Reference/mod/MmodControl.md) to account for the expected aspect ratio difference. For more information on these control types, see [Aspect ratio and aspect ratio search constraint](S12_Search_settings_specific_to_models_in_specialized_shape_contexts.md).

### CAD-file models

You can add models that are defined from CAD DXF files to a Model Finder context, using [`MmodDefineFromFile`](../../Reference/mod/MmodDefineFromFile.md). The CAD DXF files must be in the R14 file format.

Not every entity in a CAD DXF file is supported; unsupported entities are ignored. The supported entities are LINE, POLYLINE, LWPOLYLINE, CIRCLE, ARC, ELLIPSE, BLOCK (a collection of entities), and INSERT (an entity containing a reference to where the BLOCK should be drawn, and at what location and scale). Note that for the LWPOLYLINE entities, the bulge option is ignored by Aurora Imaging Library.

When the model is added to the Model Finder context from a CAD DXF file, the model will retain its coordinate system (the origin and the axis). Most CAD DXF files are oriented so that the Y-axis is positive going up, whereas the coordinate system Aurora Imaging Library uses is oriented so that the Y-axis is positive going down. So if you take the edge coordinates of an object from a CAD DXF file and put them in an image, the imaged object will look flipped when compared to the original object. This is also the case for a model defined from a CAD DXF file. This means that, unless the object is symmetrical, the match will not be made. You can use [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_CAD_Y_AXIS`](../../Reference/mod/MmodControl.md) to flip the Y-axis for the model so that it is positive going down.

### Model size, units, and camera calibration when dealing with synthetic model in a geometric or geometric-controlled context

If adding a synthetic model to an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) context, specify the model's dimensions in user-defined units. Specify the ratio between model units and pixel units using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md).

If the target is not calibrated, [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) is used for the match. If the target is calibrated, the model units should be the same as the calibrated units. If the target is calibrated, [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) will be used for draw operations, but not for the match nor the returned results.

Note that if you are searching for a synthetic model in a calibrated target, you must also associate the camera calibration context of the target with the model, using [`M_ASSOCIATED_CALIBRATION`](../../Reference/mod/MmodControl.md). Aurora Imaging Library needs the camera calibration context for internal purposes at preprocessing time. Note that Aurora Imaging Library will not use the camera calibration context to compensate for incongruencies between the model and the target, nor will Aurora Imaging Library map the model's coordinate system to that of the target.

> **Note:** After you have specified your pixel scale and scale settings, you can verify if your synthetic models are valid, using [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_VALID`](../../Reference/mod/MmodInquire.md). Synthetic models can be invalid if their global size in X or Y (_size of the model box_ x [`M_PIXEL_SCALE`](../../Reference/mod/MmodInquire.md) x [`M_SCALE`](../../Reference/mod/MmodInquire.md)) is less than 16 or greater than 1024.

By default, synthetic models in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) context are defined to have a 10% margin around the bounding box of their active edges. When finding an occurrence, extra edges found in this area will reduce the target score. You can change the size of the margin using [`MmodControl`](../../Reference/mod/MmodControl.md) with the [`M_BOX_MARGIN_...`](../../Reference/mod/MmodControl.md) control types.

### Model size, units, and camera calibration when dealing with synthetic model in a shape-specific context

If adding a synthetic model to an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) context, specify the model's dimensions in target units; the model is considered to have the same units as the target. If the target is calibrated, the model's dimensions and settings will be interpreted in world units. Conversely, if the target is not associated with a camera calibration context, the model's dimensions and settings will be interpreted in pixel units. The model will be defined in the same coordinate system as the target. You cannot associate a camera calibration context with the model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) context.

By default, synthetic models are defined to have a 10% margin around the bounding box of their active edges. For a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, this margin is only used for drawing; it does not affect the match. You can change the size of the margins using [`MmodControl`](../../Reference/mod/MmodControl.md) with the [`M_BOX_MARGIN_...`](../../Reference/mod/MmodControl.md) control types.

## Models defined from result buffers

You can define models from two different types of result buffers: Edge Finder result buffers and Model Finder result buffers. This allows you to use the edge map information in the result buffer to define the model.

In the case of these models, the **model source image** is the image from which the results were obtained. The **model source** is the result buffer in which the results are stored. The **model image** is a copy of the region in the model source image from which the model has been defined.

### Models defined from an Edge Finder result buffer

You would define models from an Edge Finder result buffer if you want to use other types of edges, such as crest edges, or if you want to re-use the edge map for another purpose. If you know that the edges of interest have a minimum size, you could use Edge Finder to include only edges above that size. The included edges of the results are the ones that will be used to define the model. These result-type models are referred to as **Edge Finder-type** models.

Some things have to be done before you can define a model from an Edge Finder result buffer.

- The Edge Finder result buffer must have been previously allocated using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md).
- The Edge Finder result buffer must be compatible with the Model Finder context. Enable this using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_MODEL_FINDER_COMPATIBLE`](../../Reference/edge/MedgeControl.md) prior to calling [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).
- You can speed up the time it takes to search for the models by setting [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md) prior to calling [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). For more information, see [Interfacing with Geometric Model Finder](../C10_Edge_Finder/S10_Interfacing_with_the_Geometric_Model_Finder_module.md).
- [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) must have been already called using this result buffer.

You can add models defined from an Edge Finder result buffer to a Model Finder context using [`MmodDefine`](../../Reference/mod/MmodDefine.md) with [`M_EDGE_RESULT`](../../Reference/mod/MmodDefine.md).

The size of the model can be defined using the offset and size parameters of [`MmodDefine`](../../Reference/mod/MmodDefine.md).

### Models defined from a Model Finder result buffer

Adding models defined from a Model Finder result buffer allows you to define new models from previous match occurrences. These models are referred to as **Model Finder-type** models.

There are many reasons to use a Model Finder-type model. For example, if you have a model that changes slightly over time, you can define a model from the result to update it. Updating the model prevents potentially misidentifying the model because it has changed beyond your starting tolerances.

To define a Model Finder-type model, use [`MmodDefine`](../../Reference/mod/MmodDefine.md) with [`M_MOD_RESULT`](../../Reference/mod/MmodDefine.md) and pass the identifier of the Model Finder result buffer and the index of the occurrence to use. The model is defined with the edges of the target at the occurrence position. You have to use [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_MOD_DEFINE_COMPATIBLE`](../../Reference/mod/MmodControl.md) enabled before you can use this feature; this must be done before [`MmodFind`](../../Reference/mod/MmodFind.md) finds the result. The result buffer must be compatible with the Model Finder context.

## Models defined from two other models

Models can be defined from two previously existing models and added to the context. Use [`MmodDefine`](../../Reference/mod/MmodDefine.md) with [`M_MERGE_MODEL`](../../Reference/mod/MmodDefine.md) and specify the two models from which to create the new model. The model will be formed by taking the first model specified and adding the edges of the second model that are not included in the edges of the first model.

There are no restrictions on the types of models you can merge. They can be of different types, as long as they are part of the same model context.

## Preprocessing

After you add models to your context and before performing the search, you have to preprocess the context, using [`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md), otherwise an error will occur. It does not matter the type of model, or the type of context. Calling [`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md) will prepare models for geometric matching. In the case of image-type models, it is also the time when edges are actually extracted from the model image.

Note that the Model Finder context must be preprocessed again if some of the models' search settings are changed. You can check if you need to preprocess after changing a control using [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_PREPROCESSED`](../../Reference/mod/MmodInquire.md).

When you save a Model Finder context, preprocessing information is not saved with the context. You must preprocess the context when the Model Finder context is restored.

## Model origin

The model origin is the point in the model that is considered to be (0,0). The location of the model origin depends on the type of model: for image and Edge Finder-type models, the model origin is the center of the upper left pixel of the model image; for predefined-shape models, the center of the shape is the model origin. The model origin for a model defined from a CAD DXF file is the same as point (0,0) in the CAD DXF file.

You can inquire the X- and Y-coordinates of a predefined-shape's model origin using [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_MODEL_IMAGE_ORIGIN_...`](../../Reference/mod/MmodInquire.md). You can inquire the X- and Y-coordinates of an image or Edge Finder-type's model origin using [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_ORIGINAL_...`](../../Reference/mod/MmodInquire.md).

You can use [`MmodControl`](../../Reference/mod/MmodControl.md) and [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_REFERENCE_...`](../../Reference/mod/MmodControl.md) to have results returned for a point other than the model origin.

## Model indices and labels

Each model added to a Model Finder context can be accessed by its index. The first model is assigned an index of 0, and for each subsequent model added, the index is increased by one. You can also apply setting changes to all models within a Model Finder context using [`M_ALL`](../../Reference/mod/MmodControl.md) as the index. When a model is deleted, all model indices greater than the deleted model are shifted down by one. Aurora Imaging Library searches for all models within a context in parallel, therefore the model index does not have any bearing in terms of a search order.

Models can also be given a user-defined numeric label, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_USER_LABEL`](../../Reference/mod/MmodControl.md). A user label can be used as a means of identifying your model, independently from its index in the Model Finder context. However, user labels cannot be used as a direct replacement for the index; to retrieve the index of a model from the user label, use [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_INDEX_FROM_LABEL`](../../Reference/mod/MmodInquire.md). The user label, once converted to an index value, can be used with any [`Mmod...`](../../Reference/mod/MmodAlloc.md) function that takes an index value. All user labels must be unique integers; that is, no two user labels can have the same integer. To remove a label from a model, set the user label to [`M_NO_LABEL`](../../Reference/mod/MmodControl.md).

## Drawing and inquiring the model's active edges

When testing different model source images for the best model candidate, you can use [`MmodDraw`](../../Reference/mod/MmodDraw.md) to draw the model's active edges. This can be done for any type of model, although for synthetic models it is less useful, since the edges should already be known. The resulting edge map can reveal whether the edges you have chosen are the ones that you want, or if there are problems with the edges. You can use [`MmodInquire`](../../Reference/mod/MmodInquire.md) with the [`M_ALLOC_SIZE...`](../../Reference/mod/MmodInquire.md) inquire types to determine the necessary size for the destination image buffer. To make corrections for image-type or Model Finder-type models, you might have to adjust the image processing control types, or mask unwanted edges. For Edge Finder-type models, you can exclude edges that you don't want from the Edge Finder result buffer and redefine the model, or you can mask unwanted edges.

If you have used offsets to define the model region in the model source, you can draw the active edges at the position where the model was defined using [`MmodDraw`](../../Reference/mod/MmodDraw.md) with [`M_ORIGINAL`](../../Reference/pat/MpatDraw.md); otherwise the active edges will be drawn at the center of the top-left pixel of the destination image. This is valid for image-type or Edge Finder-type models.

*[Image: Offsets.png]*

You can use [`MmodInquire`](../../Reference/mod/MmodInquire.md) with the [`M_CHAIN_INDEX`](../../Reference/mod/MmodInquire.md), [`M_CHAIN_X`](../../Reference/mod/MmodInquire.md), and [`M_CHAIN_Y`](../../Reference/mod/MmodInquire.md) controls to inquire the active edges of the model.

Note that if you try to draw a model's edges and its Model Finder context has not been preprocessed, a preliminary edge extraction operation will be performed for the model.

You can also draw features from a zoomed region of the model image, as well as other items. To do so, specify the appropriate values for the [`MgraControl`](../../Reference/gra/MgraControl.md)   [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md), [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md), [`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md), and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md) control types. The relative origin values must be specified in pixels, and are relative to the coordinates of the top-left corner of the region in the model image, while the scale values specify the X- and Y-scaling factors used to fill the destination image buffer.

*[Image: MmodDrawZooming.png]*
