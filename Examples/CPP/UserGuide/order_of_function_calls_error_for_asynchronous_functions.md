---
title: "order_of_function_calls_error_for_asynchronous_functions"
description: "asynchronous error generated from improper order of function calls"
ms.snippet: "userguide.order_of_function_calls_error_for_asynchronous_functions"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# order_of_function_calls_error_for_asynchronous_functions

> asynchronous error generated from improper order of function calls

**Language:** C++ | **Version:** 7.5+

```cpp
MimConvolve(DAILsystem1buffer1, DAILsystem1buffer2, M_SMOOTH);
MimConvolve(DAILsystem2buffer1, DAILsystem2buffer2, M_SMOOTH);
MthrWait(DAILsystem1, M_THREAD_WAIT, M_NULL);
AIL_INT error = MappGetError(M_DEFAULT, M_CURRENT, M_NULL);
```
