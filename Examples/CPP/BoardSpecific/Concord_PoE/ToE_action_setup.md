---
title: "ToE_action_setup"
description: "An example of setting up an action command on your Concord PoE with ToE and corresponding GigE Vision device."
ms.snippet: "boardspecific.Concord_PoE.ToE_action_setup"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# ToE_action_setup

> An example of setting up an action command on your Concord PoE with ToE and corresponding GigE Vision device.

**Language:** C++ | **Version:** 7.5+

```cpp
void SetupToEUsingActions(AIL_ID AilConcordPoESystem, const std::vector<ToEDevice>& Devices)
{
	MosPrintf(AIL_TEXT("\nSetting-up GigE Vision devices and the Zebra ConcordPoE board.\n\n"));

	/* Set-up action command on Zebra Concord PoE with ToE */
	AIL_INT DeviceKey = 0x56781234, GroupKey = 0x24, GroupMask = 0xFFFFFFFF;
	MsysControl(AilConcordPoESystem, M_GC_ACTION0 + M_GC_ACTION_DEVICE_KEY, DeviceKey);
	MsysControl(AilConcordPoESystem, M_GC_ACTION0 + M_GC_ACTION_GROUP_MASK, GroupMask);
	MsysControl(AilConcordPoESystem, M_GC_ACTION0 + M_GC_ACTION_GROUP_KEY, GroupKey);
	MsysControl(AilConcordPoESystem, M_GC_ACTION0 + M_TRIGGER_SOURCE, M_AUX_IO2);

	/* Set-up action command on the GigE Vision devices */
	for (size_t i = 0; i < Devices.size(); i++)
	{
		AIL_INT64 ActionNumber = 0;
		MdigControlFeature(Devices[i].AilDigitizerId, M_FEATURE_VALUE, AIL_TEXT("ActionSelector"), M_TYPE_INT64, &ActionNumber);
		MdigControlFeature(Devices[i].AilDigitizerId, M_FEATURE_VALUE, AIL_TEXT("ActionDeviceKey"), M_TYPE_INT64, &DeviceKey);
		MdigControlFeature(Devices[i].AilDigitizerId, M_FEATURE_VALUE, AIL_TEXT("ActionGroupMask"), M_TYPE_INT64, &GroupMask);
		MdigControlFeature(Devices[i].AilDigitizerId, M_FEATURE_VALUE, AIL_TEXT("ActionGroupKey"), M_TYPE_INT64, &GroupKey);

		MdigControlFeature(Devices[i].AilDigitizerId, M_FEATURE_VALUE, AIL_TEXT("TriggerSelector"), M_TYPE_STRING, AIL_TEXT("FrameStart"));
		MdigControlFeature(Devices[i].AilDigitizerId, M_FEATURE_VALUE, AIL_TEXT("TriggerSource"), M_TYPE_STRING, AIL_TEXT("Action0"));
		MdigControlFeature(Devices[i].AilDigitizerId, M_FEATURE_VALUE, AIL_TEXT("TriggerMode"), M_TYPE_STRING, AIL_TEXT("On"));

		MsysControl(AilConcordPoESystem, M_GC_ACTION0 + M_ADD_DESTINATION, Devices[i].AilDigitizerId);
	}
}
```
