---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_Python
section: AIL_functions_and_constants_in_Python
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Python / AIL functions and constants in Python"
---

# Aurora Imaging Library functions and constants in Python

Aurora Imaging Library functions are in the Aurora Imaging Library package; therefore, in Python, you must use the prefix `AIL.` before the names of the Aurora Imaging Library functions. The following is an example call to [`MappControl`](../../Reference/app/MappControl.md):

```
AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE)
```

> **Note:** Much like Aurora Imaging Library function calls, Aurora Imaging Library constants must also be prefixed by the chosen package name as is demonstrated above with _AIL.M_DEFAULT_, _AIL.M_ERROR_, and _AIL.M_PRINT_DISABLE_.

## Void pointers as outputs

Many Aurora Imaging Library functions have an output parameter that takes a reference to a variable. When using Python, you can omit the void pointer (for example, _*UserVarPtr_ in [`MdigInquire`](../../Reference/dig/MdigInquire.md)) and directly use the return value. The following example demonstrates this functionality in Aurora Imaging Library.

```
ProcessFrameCount = AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_COUNT)
```

By default, Aurora Imaging Library functions will automatically return what they typically return to their pointer parameters, even if there are multiple pointer parameters. The functions will return this information in the appropriate type. If you do not want a function to return information typically returned to a specific pointer parameter, set that parameter to _M_NULL_. An example of this can be seen below where [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) is used to allocate an application, a system, and a display, while the digitizer and buffer allocations are explicitly set to _M_NULL_ so that they are not allocated by default. Setting a parameter to _M_NULL_ is the only exception to omitting an output parameter that takes a reference to a variable.

```
AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr = AIL.M_NULL, ImageBufIdPtr = AIL.M_NULL)
```

## Exception handling in Aurora Imaging Library

In Python, you can use a try/except exception handling structure to implement code that will execute an except block instead of generating a runtime error in a dialog box. The type of exception thrown by Aurora Imaging Library is an **AilError**.

This is enabled using [`MappControl`](../../Reference/app/MappControl.md) with [`M_ERROR`](../../Reference/app/MappControl.md) set to[`M_THROW_EXCEPTION`](../../Reference/app/MappControl.md).

```
Mapplication = AIL.MappAlloc(AIL.M_NULL, AIL.M_DEFAULT)
AIL.MappControl(Mapplication, AIL.M_ERROR, AIL.M_THROW_EXCEPTION)
try:
  Msystem = AIL.MsysAlloc(AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, AIL.M_DEFAULT)
except AilError as exception:
  print("System Allocation Error: {0}".format(exception.Message)
```

> **Note:** In Python, runtime checks ensure the type system rules are satisfied. You can enable or disable an **AILSafeTypeError** exception to be raised if the incorrect type is provided to a pointer, with `MsafetypeEnable()` or `MsafetypeDisable()`.

### Asynchronous errors

Asynchronous errors occur independent of the current thread (for example, the camera being unplugged). To handle an asynchronous error, you can hook a function to handle the error event using the `AIL.MasyncErrorHook()` function in the Aurora Imaging Library package. To unhook the function from the asynchronous error event, use `AIL.MasyncErrorUnhook()`. Note that you should unhook the function from the asynchronous event before freeing the Aurora Imaging Library application context. This functionality is only available in Python if it was enabled using [`MappControl`](../../Reference/app/MappControl.md) with [`M_ERROR`](../../Reference/app/MappControl.md) set to [`M_THROW_EXCEPTION`](../../Reference/app/MappControl.md). The asynchronous hook function should be declared as follows in Python.

```
def AsyncErrorHookFunction(exception):
```

You can then hook the function to the error event using `AIL.MasyncErrorHook()`. An example of this is shown below.

```
AIL.MasyncErrorHook(AsyncErrorHookFunction)
```

## NumPy arrays in Python

While Python lists work in Aurora Imaging Library, they do not offer the memory efficient and speed-optimized characteristics of C-style static arrays. For improved performance, you can use NumPy arrays and pass them to parameters that take arrays. By importing NumPy, Aurora Imaging Library will automatically allocate and return a NumPy array when applicable. The following code snippet demonstrates using a preallocated NumPy array in Aurora Imaging Library.

```
import numpy
AIL.MblobCalculate(BlobContext, MimClosedestination, AIL.M_NULL, BlobResult)
NbOfBlobs = AIL.MblobGetResult(BlobResult, AIL.M_GENERAL, AIL.M_NUMBER+AIL.M_TYPE_AIL_INT)
index_values = numpy.zeros(NbOfBlobs, dtype=numpy.uint64)
AIL.MblobGetResult(BlobResult, AIL.M_INCLUDED_BLOBS, AIL.M_INDEX_VALUE, index_values)
print(index_values) #Contains the index values
```

> **Note:** If you want to use the NumPy library in your application and still want Aurora Imaging Library to return a list, you can control NumPy functionality with `MnumpyDisable()` and `MnumpyEnable()`.

## Allocating and freeing objects in Aurora Imaging Library

Unlike most memory in Python that is allocated and freed automatically, all objects in Aurora Imaging Library must be allocated and freed manually using one of the [`M...Alloc`](../../Reference/buf/MbufAlloc2d.md) and corresponding [`M...Free`](../../Reference/buf/MbufFree.md) functions. You must free all allocated application contexts, systems, buffers, displays, and any other objects that were allocated when the memory is no longer needed.

## Displaying images

To display images, you can use a standard Aurora Imaging Library display or a Tkinter display. To use a Tkinter display, allocate a display using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md) and then enable Tkinter mode, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_TK_MODE`](../../Reference/disp/MdispControl.md). Once a display is created, use [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md). For more information, see [Displaying an image in a user-defined window](../C25_Displaying_an_image/S04_Displaying_an_image_in_a_userdefined_window.md).
