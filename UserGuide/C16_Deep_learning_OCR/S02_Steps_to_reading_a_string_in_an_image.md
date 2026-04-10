---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Deep_learning_OCR
section: Steps_to_reading_a_string_in_an_image
module_tag: dlocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Deep_learning_OCR / Steps to reading a string in an image"
---

# Steps to read strings from an image

To use the Deep Learning OCR module, you can start by following the [reading steps](S02_Steps_to_reading_a_string_in_an_image.md) to read text in an image. If you are not satisfied with the results, you can follow the [fine-tuning steps](S02_Steps_to_reading_a_string_in_an_image.md) to fine-tune a classifier using images from your use-case.

## Reading steps

The following steps provide a basic methodology for using the Deep Learning OCR module to read strings in an image:

1. Allocate a Deep Learning OCR read context, using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md).
2. Allocate a Deep Learning OCR read result buffer to hold the results of the read operation, using [`MdlocrAllocResult`](../../Reference/dlocr/MdlocrAllocResult.md).
3. Set general read settings.
   1. Set the dimensions of the area that encompasses all text, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_TARGET_MAX_SIZE_X`](../../Reference/dlocr/MdlocrControl.md) and [`M_TARGET_MAX_SIZE_Y`](../../Reference/dlocr/MdlocrControl.md).
   2. If necessary, modify other context settings, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md). If you plan to add a string model from a read result, you must ensure that the context settings are appropriate to read the string at this step. For example, you can set the list of characters that are accepted by the read operation using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_ACCEPTED_CHARS`](../../Reference/dlocr/MdlocrControl.md) and adjust the maximum allowed intercharacter gap with [`M_INTERCHAR_MAX_WIDTH`](../../Reference/dlocr/MdlocrControl.md).
4. Preprocess the Deep Learning OCR read context, using [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md).
5. Perform a read operation on the specified target image, using [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) with [`M_READ_ALL`](../../Reference/dlocr/MdlocrRead.md).
6. If you want to restrict the string being read, you can define a string model.
   1. A string model can be defined from the results. Note, if the string occurrence that you intend to use for the model was improperly read, repeat steps 3.b. to 5.
      1. Draw the results, using[`MdlocrDraw`](../../Reference/dlocr/MdlocrDraw.md). You can use [`MdlocrDraw`](../../Reference/dlocr/MdlocrDraw.md) with [`M_DRAW_STRING_INDEX`](../../Reference/dlocr/MdlocrDraw.md) to see which index is associated with which string.
      2. Use the mouse to determine the index of the string occurrence at the location where you clicked ([`MdlocrGetStringIndex`](../../Reference/dlocr/MdlocrGetStringIndex.md)). Note, you can use the index from the draw operation directly and skip this step.
      3. Use [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md) to define a model from the specified string result.
      4. If necessary, adjust the settings of the string model using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) and the positional constraints stored in the string model using [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md).
   2. You can also manually define a string model using [`MdlocrDefineModel`](../../Reference/dlocr/MdlocrDefineModel.md) and adjust settings and positional constraints in the same manner.
   3. You must preprocess the Deep Learning OCR read context again, using [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md).
   4. Perform subsequent read operations on the specified target image, using [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) with [`M_MODEL_BASED`](../../Reference/dlocr/MdlocrRead.md) and the context that now contains the string model.
7. Retrieve the required results from the Deep Learning OCR read result buffer, using [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md).
8. If necessary, draw the results, using [`MdlocrDraw`](../../Reference/dlocr/MdlocrDraw.md). For example, you can specify [`M_DRAW_STRING_BOX`](../../Reference/dmr/MdmrDraw.md) to draw a bounding box around the string; then, specify [`M_DRAW_STRING`](../../Reference/dlocr/MdlocrDraw.md) to draw an annotation of the string under the bounding box.
9. If necessary, save your Deep Learning OCR read context, using [`MdlocrSave`](../../Reference/dlocr/MdlocrSave.md) or [`MdlocrStream`](../../Reference/dlocr/MdlocrStream.md).
10. Free all your allocated objects, using [`MdlocrFree`](../../Reference/dlocr/MdlocrFree.md), unless [`M_UNIQUE_ID`](../../Reference/dmr/MdmrAlloc.md) was specified during allocation.

## Fine-tuning steps

The following steps provide a basic methodology for using the Deep Learning OCR module to fine-tune a classifier:

1. Allocate a Deep Learning OCR fine-tuning context, using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md) with [`M_OCR_NET2_FINETUNE_CONTEXT`](../../Reference/dlocr/MdlocrAlloc.md).
2. Allocate a Deep Learning OCR dataset, using [`MdlocrAllocDataset`](../../Reference/dlocr/MdlocrAllocDataset.md) with [`M_OCR_NET2_DATASET`](../../Reference/dlocr/MdlocrAllocDataset.md).
3. Allocate a Deep Learning OCR fine-tuning result buffer to hold the results of the fine-tuning operation, using [`MdlocrAllocResult`](../../Reference/dlocr/MdlocrAllocResult.md) with [`M_OCR_NET2_FINETUNE_RESULT`](../../Reference/dlocr/MdlocrAllocResult.md).
4. Add results from a previous Deep Learning OCR read operation to the dataset, using [`MdlocrAddImageToDataset`](../../Reference/dlocr/MdlocrAddImageToDataset.md). You must verify that the results are correct before adding them to the dataset. If necessary, you can remove the most recently added image, using [`MdlocrDeleteLastImageFromDataset`](../../Reference/dlocr/MdlocrDeleteLastImageFromDataset.md). Alternatively, import an existing dataset from disk, using [`MdlocrImport`](../../Reference/dlocr/MdlocrImport.md).
5. If necessary, modify the fine-tuning context settings, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md). For example, you can set how much the fine-tuning context will adapt to the dataset, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_FINETUNING_LEVEL`](../../Reference/dlocr/MdlocrControl.md) and adjust the number of images in the fine-tuning dataset that can be processed at the same time with [`M_MINI_BATCH_SIZE`](../../Reference/dlocr/MdlocrControl.md).
6. Preprocess the Deep learning OCR fine-tuning context, using [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md).
7. Perform a fine-tuning operation using [`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md). This process can be time intensive depending on the number of images in the dataset and specific hardware limitations.
8. Copy the fine-tuning result from the Deep Learning OCR fine-tuning result buffer to the read context, using [`MdlocrCopy`](../../Reference/dlocr/MdlocrCopy.md).
9. Preprocess the Deep learning OCR read context, using [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md).
10. Evaluate the performance of the fine-tuned classifier by performing read operations with the fine-tuned classifier, using [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md).
11. Free all your allocated objects, using [`MdlocrFree`](../../Reference/dlocr/MdlocrFree.md), unless [`M_UNIQUE_ID`](../../Reference/dmr/MdmrAlloc.md) was specified during allocation.
