---
title: "cs"
description: "userguide.mil_in_net.delegate_userdata"
ms.snippet: "userguide.ail_in_net.delegate_userdata.cs"
ms.language: "C#"
ms.env: "cs"
ms.version: "7.5"
---

# cs

> userguide.mil_in_net.delegate_userdata

**Language:** C# | **Version:** 7.5+

```csharp
using System;
using System.Runtime.InteropServices;
using Zebra.AuroraImagingLibrary;

namespace Delegates.UserData
{
    class MyUserData
    {
        public AIL_INT MyValue;
    }

    class MyDigitizer
    {
        private AIL_ID _digitizerId = AIL.M_NULL;
        private AIL_DIG_HOOK_FUNCTION_PTR _myDelegate;
        private MyUserData _myUserData;
        private GCHandle _userDataHandle;

        public MyDigitizer(AIL_ID sysId)
        {
            AIL.MdigAlloc(sysId, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, ref _digitizerId);
        }

        public void Hook()
        {
            // Prepare user data object
            _myUserData = new MyUserData();
            _myUserData.MyValue = 10;

            // Allocate a regular GCHandle to inform the garbage collector
            // not to move the object.
            _userDataHandle = GCHandle.Alloc(_myUserData);

            _myDelegate = new AIL_DIG_HOOK_FUNCTION_PTR(MyHookHandler);

            AIL.MdigHookFunction(_digitizerId,
                                 AIL.M_GRAB_START,
                                 _myDelegate,
                                 GCHandle.ToIntPtr(_userDataHandle));
        }

        public AIL_INT MyHookHandler(AIL_INT hookType, AIL_ID eventId, IntPtr userDataPtr)
        {
            if (userDataPtr != IntPtr.Zero)
            {
                // Use this to get the user data object reference from the GCHandle
                GCHandle userDataHandle = GCHandle.FromIntPtr(userDataPtr);
                MyUserData userData = userDataHandle.Target as MyUserData;

                if (userData.MyValue == 10)
                {
                    // do something
                }
            }

            return AIL.M_NULL;
        }

        public void Unhook()
        {
            // Unhook the function in Aurora Imaging Library.
            AIL.MdigHookFunction(_digitizerId,
                                 AIL.M_GRAB_START + AIL.M_UNHOOK,
                                 _myDelegate,
                                 GCHandle.ToIntPtr(_userDataHandle));

            // Free the GCHandle when no longer needed
            _userDataHandle.Free();
        }
    }
}
```
