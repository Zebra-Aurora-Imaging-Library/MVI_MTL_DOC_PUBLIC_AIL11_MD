---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Automatically_detecting_the_code_type
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Automatically detecting the code type"
---

# Automatically detecting the code type

You can automatically detect the code type, encoding scheme, and position of most 1D code occurrences in an image, using [`McodeDetect`](../../Reference/code/McodeDetect.md). You can use this information to automatically or manually set up your code models with an appropriate code type and encoding scheme for the code occurrences in your images. Detecting the code type is especially useful when it is unknown because you need this information to add a code model. In addition, being specific about the encoding scheme, rather than [`M_ANY`](../../Reference/code/McodeControl.md), can accelerate [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations. You can use the positional results for graphical and debugging purposes.

[`McodeDetect`](../../Reference/code/McodeDetect.md) supports all 1D code types, except postal, GS1 Databar, and Pharmacode code types. It cannot detect 2D or composite code types.

Note that some code types are a subset of another. If a code occurrence has only the common features of these code types, [`McodeDetect`](../../Reference/code/McodeDetect.md)cannot distinguish if the code occurrence is of one type or the other at decode-time. In these cases, [`McodeDetect`](../../Reference/code/McodeDetect.md) reports the following:

| When the following code types cannot be differentiated | Code type reported |
| --- | --- |
| [`M_UPC_A`](../../Reference/code/McodeModel.md) and [`M_EAN13`](../../Reference/code/McodeModel.md) | [`M_UPC_A`](../../Reference/code/McodeModel.md) |
| [`M_EAN14`](../../Reference/code/McodeModel.md), [`M_GS1_128`](../../Reference/code/McodeModel.md), and [`M_CODE128`](../../Reference/code/McodeModel.md) | [`M_EAN14`](../../Reference/code/McodeModel.md) |
| [`M_GS1_128`](../../Reference/code/McodeModel.md) and [`M_CODE128`](../../Reference/code/McodeModel.md) | [`M_GS1_128`](../../Reference/code/McodeModel.md) |

You can remove this ambiguity by restricting the possible code types when calling [`McodeDetect`](../../Reference/code/McodeDetect.md). Another reason to restrict the possible code types is if you are not interested in detecting certain code types; this will accelerate [`McodeDetect`](../../Reference/code/McodeDetect.md). Note that [`McodeDetect`](../../Reference/code/McodeDetect.md) does not take a code context. All information must be passed directly to the function.

When you call [`McodeDetect`](../../Reference/code/McodeDetect.md), you must specify the total number of code occurrences to find in the specified image, regardless of their code type. The function detects the specified number of code occurrences with the best quality. If it detects less than the specified number, [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_STATUS`](../../Reference/code/McodeGetResult.md)returns [`M_STATUS_DETECT_FAILED`](../../Reference/code/McodeGetResult.md).

To automatically add code models to a code context using the results of [`McodeDetect`](../../Reference/code/McodeDetect.md), use [`McodeModel`](../../Reference/code/McodeModel.md) with [`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md). This first deletes all existing code models in the code context, and then adds a code model for each detected code type. If two code occurrences with the same code type but different encoding schemes are detected, only one code model of this type is added and its encoding scheme is set to [`M_ANY`](../../Reference/code/McodeModel.md) (if supported by the code type).

The _CodeDetect.cpp_ example shows you how to automatically add code models to a code context using the results of [`McodeDetect`](../../Reference/code/McodeDetect.md). It also shows you how to annotate a displayed image with the location and name of the detected code occurrences. To run this and other examples, use Aurora Imaging Example Launcher.

If you don't want to delete the existing models in the context, you can manually add a code model to a code context using the results of [`McodeDetect`](../../Reference/code/McodeDetect.md). To do so, perform the following:

1. Retrieve the code type and encoding scheme of a detected code occurrence using [`McodeGetResult`](../../Reference/code/McodeGetResult.md), first with [`M_CODE_TYPE`](../../Reference/code/McodeGetResult.md)and then with [`M_ENCODING`](../../Reference/code/McodeGetResult.md), respectively.
2. Call [`McodeModel`](../../Reference/code/McodeModel.md) with [`M_ADD`](../../Reference/code/McodeModel.md), the code context, and the code type; this adds a code model of the specified code type to the context. Note that [`McodeDetect`](../../Reference/code/McodeDetect.md) can find multiple occurrences of the same code type and encoding scheme. Instead of adding multiple code models of the same code type and encoding scheme, increment the number of occurrences to find of that code model, using [`McodeControl`](../../Reference/code/McodeControl.md) with the code model index and [`M_NUMBER`](../../Reference/code/McodeControl.md).
3. Set the encoding scheme of the newly added code model using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_ENCODING`](../../Reference/code/McodeControl.md).

Repeat the process above for all occurrences detected. Note that you can also use[`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_ALL`](../../Reference/code/McodeGetResult.md)to retrieve the above mentioned results; this retrieves the specified result for all code occurrences at once.
