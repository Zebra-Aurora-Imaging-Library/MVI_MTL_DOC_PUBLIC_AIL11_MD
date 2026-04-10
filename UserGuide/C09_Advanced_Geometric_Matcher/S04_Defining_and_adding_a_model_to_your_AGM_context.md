---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_Geometric_Matcher
section: Defining_and_adding_a_model_to_your_AGM_context
module_tag: agm
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-geometric-matcher / Defining and adding a model to your AGM context"
---

# Defining and adding a model to your AGM context

After allocating an AGM context using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md), you can add a model to it using [`MagmDefine`](../../Reference/agm/MagmDefine.md). Since AGM does not support searching for occurrences with a different scale than the model, you should ensure your model has a similar scale as your expected occurrences.

There are two types of models in the AGM module, single-definition ([`M_SINGLE`](../../Reference/agm/MagmDefine.md)) and composite-definition ([`M_COMPOSITE`](../../Reference/agm/MagmDefine.md)). A single-definition model is defined from a single appearance of the model to find. A composite-definition model is defined from multiple model appearances and background areas. A single-definition model does not require training, while a composite-definition model is trained using images within which regions are identified as positive (true model occurrences) or negative (clearly non-model regions or potential false positives). Both types of models support finding occurrences with slight variations and finding occurrences on a cluttered background.

## Single-definition models

A single-definition model is defined from a single appearance of the model to find. An [`MagmFind`](../../Reference/agm/MagmFind.md) operation performed with a single-definition model is typically faster than with a composite-definition model. Using a single-definition model is also simpler than using a composite-definition model since it does not need training. You should start with a single-definition model and use a composite-definition model only if you find false occurrences.

You can define a single-definition model from a CAD DXF file, an Edge Finder result buffer, or a single source image. In the latter case, the entire image is used as the model or the model is extracted from a rotated or non-rotated region.

### Defining a single-definition model from a CAD DXF file

You can add a single-definition model that is defined from a CAD DXF file to a find AGM context, using [`MagmDefine`](../../Reference/agm/MagmDefine.md) with [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

Not every entity in a CAD DXF file is supported; unsupported entities are ignored. The supported entities are LINE, ARC, CIRCLE, ELLIPSE, INSERT, POLYLINE, and LWPOLYLINE (including bulge information). For INSERT entities, only single insertions are supported; multiple insertions using the MINSERT command are not supported. Extrusion is also not supported.

When the model is added to the find AGM context from a CAD DXF file, the model will retain its coordinate system (the origin and the axis). Most CAD DXF files are oriented so that the Y-axis is positive going up, whereas the coordinate system Aurora Imaging Library uses is oriented so that the Y-axis is positive going down. So if you take the edge coordinates of an object from a CAD DXF file and put them in an image, the imaged object will look flipped when compared to the original object. This is also the case for a model defined from a CAD DXF file. This means that, unless the object is symmetrical, the match will not be made. You can use [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_CAD_Y_AXIS`](../../Reference/agm/MagmControl.md) to flip the Y-axis for the model so that it is positive going down.

### Defining a single-definition model from an Edge Finder result buffer

You can define a single-definition model from the results of an edge extraction performed using Edge Finder. The included edges of the results are the ones that will be used to define the model.

Some things have to be done before you can define a model from an Edge Finder result buffer.

- The Edge Finder result buffer must have been previously allocated using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md).
- The Edge Finder result buffer must be compatible with the AGM context. Enable this using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_AGM_COMPATIBLE`](../../Reference/edge/MedgeControl.md) prior to calling [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).
- [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be set to 1 (default); otherwise, you will get an Aurora Imaging Library error.
- [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) must have been already called using this result buffer.

See [Interfacing with Geometric Model Finder or AGM](../C10_Edge_Finder/S10_Interfacing_with_the_Geometric_Model_Finder_module.md) for more information.

You can add a single-definition model that is defined from an Edge Finder result buffer to a find AGM context, using [`MagmDefine`](../../Reference/agm/MagmDefine.md) with [`M_EDGE_RESULT`](../../Reference/agm/MagmDefine.md).

### Defining a single-definition model from an image

You can add a single-definition model that is defined from an image to a find AGM context, using [`MagmDefine`](../../Reference/agm/MagmDefine.md) with [`M_IMAGE`](../../Reference/agm/MagmDefine.md) or [`M_IMAGE_REGION`](../../Reference/agm/MagmDefine.md). The entire source image is used as the model or the model is extracted from a region of the source image, respectively. Note that the region can be rotated.

The specified source image must be a 1-band, 8-bit unsigned image. The minimum size of a source image is limited to 6 x 6 pixels, while the maximum size of 32768 x 32768 pixels is permitted.

*[Image: AGM_circuit_pins_single_model.png]*

The model's source image should be of the best quality possible. If noisy, try to clean the image using image processing techniques discussed elsewhere in this user guide.

When using the entire source image as the model, it must not have a region of interest (ROI) associated with it.

To extract the model from a region of the source image, you must specify a source image that is associated with a rectangular ROI. To define the ROI, add a rectangle graphic to a 2D graphics list, using [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md). To set that rectangle graphic as the ROI of the model source image, you can call [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with either [`M_NO_RASTERIZE`](../../Reference/buf/MbufSetRegion.md) or [`M_RASTERIZE`](../../Reference/buf/MbufSetRegion.md), producing an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI or an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, respectively. Note that the center of the rectangle must be inside the model source image.

## Composite-definition models

A composite-definition model is built from training images, where instances of the model and background areas are identified with blue and red rectangles in the ROIs of the training images, respectively. While both single-definition and composite-definition models can find occurrences with variations and occurrences in cluttered targets, composite-definition models excel at these tasks because they learn to recognize varied model appearances and to ignore clutter in the background. If there is an item in the target that resembles your model, searching with a single-definition model might result in that item falsely identified as an occurrence. To avoid this, use a composite-definition model.

To add a composite-definition model to your train AGM context, use [`MagmDefine`](../../Reference/agm/MagmDefine.md) with [`M_COMPOSITE`](../../Reference/agm/MagmDefine.md). Then, train the model with training images using [`MagmTrain`](../../Reference/agm/MagmTrain.md). Before training the model, you must label the positive and negative model locations in the training images; you can use the AGM labeling tool to interactively draw the rectangles and automatically define the ROI. Note that all rectangles must be the same size. Rectangles must have an angle of 0.0°, unless you set [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_TRAIN_DETECTION_ANGLE`](../../Reference/agm/MagmControl.md) to [`M_ENABLE`](../../Reference/agm/MagmControl.md). If you set [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_NEGATIVE_LABELS_MODE`](../../Reference/agm/MagmControl.md) to [`M_AUTO`](../../Reference/agm/MagmControl.md), [`MagmTrain`](../../Reference/agm/MagmTrain.md) will automatically identify and label the negative model locations. In this case, you must not add any red rectangles to your training images. For more information about how to label your training images and about the AGM labeling tool, see [Labeling training images](S07_Training_a_model.md).

Once trained, you copy the trained composite-definition model to a find AGM context, using [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md) with [`M_TRAINED_MODEL`](../../Reference/agm/MagmCopyResult.md).

## Preprocessing

After you add (or copy) a model to your AGM context and before performing an [`MagmFind`](../../Reference/agm/MagmFind.md) or [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation, you have to preprocess the context, using [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md); otherwise, an error will occur. It does not matter the type of model or the type of context. Calling [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md) will prepare the model for matching or training. In the case of single-definition models defined from an image, it is also the time when edges are actually extracted from the model image.

Note that the AGM context must be preprocessed again if some of the model's search settings are changed. You can check if you need to preprocess after changing a control using [`MagmInquire`](../../Reference/agm/MagmInquire.md) with [`M_PREPROCESSED`](../../Reference/agm/MagmInquire.md).

When you save an AGM context, preprocessing information is not saved with the context. You must preprocess the context when the AGM context is restored.

## Model origin

The model origin is the point in the model that is considered to be (0,0). The model origin for a model defined from a CAD DXF file is the same as point (0,0) in the CAD DXF file. For all other types of models, the location of the model origin is the center of the model.

You can use [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_REFERENCE_...`](../../Reference/agm/MagmControl.md) to have results returned for a point other than the model origin.
