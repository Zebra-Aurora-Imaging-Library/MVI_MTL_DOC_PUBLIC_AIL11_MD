---
title: "ToE_software_setup"
description: "An example of setting up a software trigger on your Concord PoE with ToE and corresponding GigE Vision device."
ms.snippet: "boardspecific.Concord_PoE.ToE_software_setup"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# ToE_software_setup

> An example of setting up a software trigger on your Concord PoE with ToE and corresponding GigE Vision device.

**Language:** C++ | **Version:** 7.5+

```cpp
void SetupToEUsingSoftware(AIL_ID AilConcordPoESystem, const std::vector<ToEDevice>& Devices)
{
	MosPrintf(AIL_TEXT("\nSetting-up GigE Vision devices and the Zebra ConcordPoE board.\n\n"));

	/* Set-up GigE software trigger on Zebra Concord PoE with ToE */
	MsysControl(AilConcordPoESystem, M_GC_TRIGGER_SOFTWARE0 + M_TRIGGER_SOURCE, M_TIMER1);
	MsysControl(AilConcordPoESystem, M_GC_TRIGGER_SOFTWARE0 + M_GC_TRIGGER_SELECTOR, AIL_TEXT("FrameStart"));

	/* Set-up GigE software trigger on the GigE Vision devices */
	for (size_t i = 0; i < Devices.size(); i++)
	{
		MdigControlFeature(Devices[i].AilDigitizerId, M_FEATURE_VALUE, AIL_TEXT("TriggerSelector"), M_TYPE_STRING, AIL_TEXT("FrameStart"));
		MdigControlFeature(Devices[i].AilDigitizerId, M_FEATURE_VALUE, AIL_TEXT("TriggerSource"), M_TYPE_STRING, AIL_TEXT("Software"));
		MdigControlFeature(Devices[i].AilDigitizerId, M_FEATURE_VALUE, AIL_TEXT("TriggerMode"), M_TYPE_STRING, AIL_TEXT("On"));

		MsysControl(AilConcordPoESystem, M_GC_TRIGGER_SOFTWARE0 + M_ADD_DESTINATION, Devices[i].AilDigitizerId);
	}

	MsysControl(AilConcordPoESystem, M_GC_TRIGGER_SOFTWARE0 + M_TRIGGER_STATE, M_ENABLE);
}
```
