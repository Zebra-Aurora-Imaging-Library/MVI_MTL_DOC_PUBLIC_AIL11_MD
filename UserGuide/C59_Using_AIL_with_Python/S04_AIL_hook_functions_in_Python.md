---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_Python
section: AIL_hook_functions_in_Python
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Python / AIL hook functions in Python"
---

# Aurora Imaging Library hook-type functions in Python

In Aurora Imaging Library, there are functions (for example, [`MappHookFunction`](../../Reference/app/MappHookFunction.md)) that take a callback function and set it as the event handler for a specific type of event (hook-handler function hooks to the event). When the event occurs, the callback function is called. The Aurora Imaging Library functions capable of hooking a callback function to an event are referred to as hook-type functions. Hook-type functions take as input a reference to the callback function and the name of the event that should invoke the execution of the callback function. The reference to the callback function must be of a specific callback type. The following table lists the Aurora Imaging Library hook-type functions and their associated callback function type.

| Aurora Imaging Library hook-type function | Callback type |
| --- | --- |
|  |
| [`MappHookFunction`](../../Reference/app/MappHookFunction.md) | AIL_APP_HOOK_FUNCTION_PTR |
| [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) | AIL_BUF_HOOK_FUNCTION_PTR |
| [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md) | AIL_CLASS_HOOK_FUNCTION_PTR |
| [`McomHookFunction`](../../Reference/com/McomHookFunction.md) | AIL_COM_HOOK_FUNCTION_PTR |
| [`MdigFocus`](../../Reference/dig/MdigFocus.md) | AIL_FOCUS_HOOK_FUNCTION_PTR |
| [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md), [`MdigProcess`](../../Reference/dig/MdigProcess.md) | AIL_DIG_HOOK_FUNCTION_PTR |
| [`MdispHookFunction`](../../Reference/disp/MdispHookFunction.md) | AIL_DISP_HOOK_FUNCTION_PTR |
| [`MfpgaHookFunction`](../../Reference/fpga/MfpgaHookFunction.md) | AIL_FPGA_HOOK_FUNCTION_PTR |
| [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) | AIL_FUNC_FUNCTION_PTR |
| [`MgraHookFunction`](../../Reference/gra/MgraHookFunction.md) | AIL_GRA_HOOK_FUNCTION_PTR |
| [`McomHookFunction`](../../Reference/com/McomHookFunction.md) | AIL_OBJ_HOOK_FUNCTION_PTR |
| [`MobjHookFunction`](../../Reference/obj/MobjHookFunction.md) | AIL_OCR_HOOK_FUNCTION_PTR |
| [`MseqHookFunction`](../../Reference/seq/MseqHookFunction.md) | AIL_SEQ_HOOK_FUNCTION_PTR |
| [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) | AIL_SYS_HOOK_FUNCTION_PTR |
| [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) | AIL_THREAD_FUNCTION_PTR |

In Python, you can also pass objects directly to hook functions. This is done by defining a Python object and passing it directly to the function. An example of this is shown below.

```
class HookDataStruct( ):
  def __init__ (self, AilDigitizer, AilImageDisp, ProcessedImageCount):
    self.AilDigitizer = AilDigitizer
    self.AilImageDisp = AilImageDisp
    self.ProcessedImageCount = ProcessedImageCount 
    #... 
UserHookData = HookDataStruct(AilDigitizer, AilImageDisp, 0)
ProcessingFunctionPtr = AIL.AIL_DIG_HOOK_FUNCTION_PTR(ProcessingFunction)
AIL.MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize, AIL.M_START, AIL.M_DEFAULT, ProcessingFunctionPtr, UserHookData) 
```

The callback function must meet certain requirements. For this information, refer to the description of the corresponding hook-type function.

In the example above, [`MdigProcess`](../../Reference/dig/MdigProcess.md) hooks the callback function _ProcessingFunction_ to the end of each grabbed image. [`MdigProcess`](../../Reference/dig/MdigProcess.md) requires that its callback function is declared as follows in Python:

```
def ProcessingFunction(HookType, EventId, HookDataPtr):
```

Where for this example, _ProcessingFunction_ is the callback function, _HookType_ is the type of event that will invoke the call, _EventId_ is the identifier of the event, and _HookDataPtr_ is a pointer to the user data that will be passed to the callback function when it is invoked.
