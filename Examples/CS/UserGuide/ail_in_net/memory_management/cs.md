---
title: "cs"
description: "pinning memory used by MbufCreate2D"
ms.snippet: "userguide.ail_in_net.memory_management.cs"
ms.language: "C#"
ms.env: "cs"
ms.version: "7.5"
---

# cs

> pinning memory used by MbufCreate2D

**Language:** C# | **Version:** 7.5+

```csharp
using System.Runtime.InteropServices;
using Zebra.AuroraImagingLibrary;

namespace MemoryManagement
{
    class MappedBuffer2d
    {
        private AIL_ID _bufferId = AIL.M_NULL;
        private GCHandle _handle;

        public MappedBuffer2d(AIL_ID systemId, int sizeX, int sizeY, byte[,] imageData)
        {
            _handle = GCHandle.Alloc(imageData, GCHandleType.Pinned);

            // cast the returned address to a ulong because MbufCreate2d expects a ulong
            ulong addressOfImageData = (ulong)_handle.AddrOfPinnedObject();
            AIL.MbufCreate2d(systemId,
                             sizeX,
                             sizeY,
                             8 + AIL.M_UNSIGNED,
                             AIL.M_IMAGE + AIL.M_DISP,
                             AIL.M_HOST_ADDRESS,
                             sizeX,
                             addressOfImageData,
                             ref _bufferId);
        }

        public void Free()
        {
            if (_bufferId != AIL.M_NULL)
            {
                AIL.MbufFree(_bufferId);
                _handle.Free();
            }
        }
    }
}
```
