---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Prediction
section: ONNX
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / ML_Predicting_and_advanced_techniques / ONNX"
---

# ONNX

ONNX (Open Neural Network Exchange) is an open-source format that lets you create, train, and save a machine learning model. The Classification module lets you import such a model into an ONNX classifier context ([`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md)) and incorporate it into your Aurora Imaging Library application for the inference ([`MclassPredict`](../../Reference/class/MclassPredict.md)). The imported ONNX model is already trained (no further training is required).

The classification task that you can perform with an ONNX classifier depends on the machine learning model you designed and trained prior to importing it. Possible tasks include those inherently available with the Classification module (for example, image classification, segmentation, object detection, and anomaly detection), as well as any other task available with ONNX.

The following steps provide a basic methodology for using an ONNX classifier:

1. Allocate an ONNX classifier context, using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md).
2. Import a trained ONNX machine learning model into an ONNX classifier context, using [`MclassImport`](../../Reference/class/MclassImport.md) with [`M_ONNX_FILE`](../../Reference/class/MclassImport.md).
3. Optionally, modify ONNX classifier settings, using [`MclassControl`](../../Reference/class/MclassControl.md).
4. Preprocess the ONNX classifier context, using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).
5. Allocate an ONNX result buffer to hold the prediction results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_ONNX_RESULT`](../../Reference/class/MclassAllocResult.md).
6. Perform the inference using the imported ONNX classifier on the specified target image, using [`MclassPredict`](../../Reference/class/MclassPredict.md).
7. Retrieve the required results from the classification result buffer, using [`MclassGetResult`](../../Reference/class/MclassGetResult.md).
8. Free all your allocated objects, using [`MclassFree`](../../Reference/class/MclassFree.md), unless [`M_UNIQUE_ID`](../../Reference/class/MclassAlloc.md) was specified during allocation.

## ONNX requirements

To ensure the ONNX machine learning model that you import with [`M_ONNX_FILE`](../../Reference/class/MclassImport.md) works as expected, all per-established requirements must be met. For example, you must ensure:

- You are using a supported ONNX file format version (ir_version).
- The network's inputs and outputs are properly configured. This includes the network having at least 1 input and 1 output, and that the type of the inputs and outputs are dense tensor (the tensors must have a shape).
- The graph of the machine learning model is free from cycles.

For a complete description of all requirements, refer to [`M_ONNX_FILE`](../../Reference/class/MclassImport.md) in the Aurora Imaging Library Reference.
