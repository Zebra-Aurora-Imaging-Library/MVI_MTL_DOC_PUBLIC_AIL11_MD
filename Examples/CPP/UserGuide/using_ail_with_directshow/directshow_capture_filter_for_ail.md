---
title: "directshow_capture_filter_for_ail"
description: "Adding rectangle interactively"
ms.snippet: "userguide.using_ail_with_directshow.directshow_capture_filter_for_ail"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# directshow_capture_filter_for_ail

> Adding rectangle interactively

**Language:** C++ | **Version:** 7.5+

```cpp
/* See DirectShow Reference on MSDN for more information on any
   functions and methods used in this example specific to DirectShow. */

IAilCapture*      pAilCapture    = NULL;  /* Aurora Imaging Library system DirectShow interface.    */
IAilCapturePin*   pAilCapturePin = NULL;  /* Aurora Imaging Library digitizer DirectShow interface. */
IGraphBuilder*    pGraphBuilder  = NULL;  /* DirectShow Graph building utility.  */

HRESULT hr = S_OK; /* HRESULT variable that stores the outcome of an operation */

/* Creates a filter graph object to store filters */
hr = CoCreateInstance(  CLSID_FilterGraph,
                        NULL,
                        CLSCTX_INPROC_SERVER,
                        IID_IGraphBuilder,
                        (void**)&pGraphBuilder );

/* Creates the Aurora Imaging Library Capture filter object */
hr = CoCreateInstance(  CLSID_AilCapture,
                        NULL,
                        CLSCTX_INPROC_SERVER,
                        IID_IAilCapture,
                        (void**)&pAilCapture );

/* Set Aurora Imaging Library system information before the system is allocated. An Aurora Imaging Library 
   system is allocated internally when the filter is added to the filter graph. */
pAilCapture->setSystemInfo( AIL_TEXT("M_SYSTEM_DEFAULT"), M_DEFAULT );
pGraphBuilder->AddFilter( pAilCapture, NULL );

/* Find a pin on the Aurora Imaging Library Capture Filter. Only one pin is available when the filter
   is initially added to the filter graph. A new pin is created when the first pin 
   is connected to another filter, only if there are digitizers avalable. */
IPin* pIPin = NULL;
pAilCapture->FindPin( L"default", &pIPin );

/* Queries the IID_IAilCapturePin interface to expose the IAilCapturePin methods. */
hr = pIPin->QueryInterface( IID_IAilCapturePin, (void**)&pAilCapturePin );
```
