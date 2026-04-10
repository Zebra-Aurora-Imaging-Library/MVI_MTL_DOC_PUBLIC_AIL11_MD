---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Deep_learning_OCR
section: Finetuning
module_tag: dlocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Deep_learning_OCR / Finetuning"
---

# Fine-tuning

Fine-tuning is the process of taking a pretrained classifier and continuing training on a dataset, specific to your use case. If you are not satisfied with the results from one of the predefined Deep Learning OCR read contexts, you can fine-tune the classifier to improve read results. Fine-tuning is only available for contexts using the NET2 architecture.

To fine-tune a Deep Learning OCR context, use [`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md). This function will store the results of the fine-tuning operation in a Deep Learning OCR fine-tuning result buffer ([`M_OCR_NET2_FINETUNE_RESULT`](../../Reference/dlocr/MdlocrAllocResult.md)).

## Fine-tuning context and result buffer

A fine-tuning context contains the training engine and all of the settings required to fine-tune a classifier. You can allocate a Deep Learning OCR fine-tuning context, using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md) with [`M_OCR_NET2_FINETUNE_CONTEXT`](../../Reference/dlocr/MdlocrAlloc.md). The results of a fine-tuning operation ([`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md)) are stored in a Deep Learning OCR result buffer. The results of the fine-tuning operation can be copied from the Deep Learning OCR result buffer to a Deep Learning OCR read context. The read context must have been previously allocated, using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md) with [`M_OCR_NET2_TINY_V1`](../../Reference/dlocr/MdlocrAlloc.md).

## Context settings

Some settings can be controlled, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) to customize the fine-tuning operations.

For setting modifications to take effect, you must preprocess the fine-tuning context by calling [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md) before performing the fine-tuning operation.

### Fine-tuning level

You can set how much the fine-tuning context will adapt to the dataset, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_FINETUNING_LEVEL`](../../Reference/dlocr/MdlocrControl.md). By default, the fine-tuning level is set to [`M_LOW`](../../Reference/dlocr/MdlocrControl.md). Lower fine-tuning levels allow you to add results from your use case into the training data of the classifier without losing the ability to generalize to different text. With higher fine-tuning levels, the classifier becomes more adapted to the provided dataset, making it more accurate for the specific use case. However, this can impact its performance outside of the use cases presented in the dataset. The performance of different fine-tuning levels should be tested on your particular use case and machine.

### Mini-batch size

Datasets cannot usually fully reside in memory during training. Mini-batches are used to break down the dataset that each epoch cycles through. Aurora Imaging Library loads the data and processes it one mini-batch after another for each epoch.

The mini-batch size ([`M_MINI_BATCH_SIZE`](../../Reference/dlocr/MdlocrControl.md)) determines the number of images that are part of a mini-batch. A larger mini-batch size results in a faster training process and better accuracy, but requires more memory.

The maximum batch size is limited by the available memory for calculations. In general, a bigger batch size improves the classifier's accuracy, however, depending on data, it is not systematically the case.

Batch size plays a key role in the consumption of resources. It is recommended to observe memory (GPU or CPU) during fine-tuning with the help of the operating system's performance monitor.

## Stopping a fine-tuning operation

You can halt the fine-tuning operation by calling [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_STOP_READ`](../../Reference/dlocr/MdlocrControl.md) on the fine-tuning result buffer. No results are returned if the fine-tuning operation is stopped.

## Building a dataset

In order to fine-tune a Deep Learning OCR read context, you must first build a dataset. A dataset must contain only valid read results obtained from previous read operations ([`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md)). You can add these results to the dataset, using [`MdlocrAddImageToDataset`](../../Reference/dlocr/MdlocrAddImageToDataset.md). If necessary, you can remove the most recently added result from the dataset, using [`MdlocrDeleteLastImageFromDataset`](../../Reference/dlocr/MdlocrDeleteLastImageFromDataset.md). The outcome of the fine-tuning operation heavily depends on the number and validity of the results added to the dataset. In general, a larger dataset with valid results from your particular use-case will result in a more robust classifier.

### Importing and exporting datasets

Once you have added results to your dataset, you can export it for future use, using [`MdlocrExport`](../../Reference/dlocr/MdlocrExport.md). This function creates a copy of all of the images in the dataset and creates a JSON file containing information about the dataset in the same folder. You can correct any errors in the dataset by modifying the JSON file.

You can import a dataset that was previously exported, using [`MdlocrImport`](../../Reference/dlocr/MdlocrImport.md). You must import the images and result into an existing Deep Learning OCR dataset. Setting the [`Operation`](../../Reference/dlocr/MdlocrImport.md) parameter to [`M_APPEND`](../../Reference/dlocr/MdlocrImport.md) will write the imported entries after the existing entries in the dataset. Setting the [`Operation`](../../Reference/dlocr/MdlocrImport.md) parameter to [`M_RESTORE`](../../Reference/dlocr/MdlocrImport.md) will delete any existing entries in the dataset then add the entries from disk.

### Adding a progress bar

Fine-tuning operations can be time-intensive, so it can be useful to know how much progress has been made. To create a progress bar, you can hook a function to a Deep Learning OCR fine-tuning event, using [`MdlocrHookFunction`](../../Reference/dlocr/MdlocrHookFunction.md) with [`M_FINETUNE_PROGRESS`](../../Reference/dlocr/MdlocrHookFunction.md). The hook-handler function will be called every time progress is made in the fine-tuning process. You can use [`MdlocrGetHookInfo`](../../Reference/dlocr/MdlocrGetHookInfo.md) with [`M_PERCENTAGE`](../../Reference/dlocr/MdlocrGetHookInfo.md) to retrieve the completion percentage of the fine-tuning operation.

## Examples

The _DlocrFinetune.cpp_ example uses the Deep Learning OCR module to fine-tune a Deep learning OCR read context on a Deep Learning OCR dataset. The _DlocrPreAnnotation.cpp_ example uses the Deep Learning OCR module to read a set of images and export the results as a preannotated Deep Learning OCR dataset. To run this and other examples, use Aurora Imaging Example Launcher.
