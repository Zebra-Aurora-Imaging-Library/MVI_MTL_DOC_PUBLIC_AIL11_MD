---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo-CoF
section: Using_Hook_Data
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cof / Using Hook Data"
---

# Using Zebra Rapixo CoF data latches

Zebra Rapixo CoF has data latches to store signal state information, the quadrature decoder's counter value, or a timestamp at a specific point during a grab. Data latches are reset at every end of grabbed frame event. Data latches do not suffer the delays typical to hooking a hook-handler function to these specific points during the grab and inquiring and storing related information using software.

*[Image: radient_data_latches_hardaware_concept.png]*

> **Note:** Data latches are only available when using an Aurora Imaging Library digitizer allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEV0`](../../Reference/dig/MdigAlloc.md). Zebra Rapixo CoF has 16 data latches for use with a digitizer allocated with [`M_DEV0`](../../Reference/dig/MdigAlloc.md).

You can specify the data latch trigger source and the data to record upon the trigger using [`MdigControl`](../../Reference/dig/MdigControl.md) with the [`M_DATA_LATCH...`](../../Reference/dig/MdigControl.md) control types.

To retrieve data from a data latch, use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md)with [`M_DATA_LATCH...`](../../Reference/dig/MdigGetHookInfo.md).

## Steps to configure and retrieve data from a data latch

The following steps provide a basic methodology for configuring and retrieving data from a data latch:

1. Specify what triggers storing the specified information to the specified data latch, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_DATA_LATCH_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) + [`M_LATCHn`](../../Reference/dig/MdigGetHookInfo.md). The trigger can be a specific auxiliary signal, a quadrature decoder's counter value, a timer active event, or one of a limited number of grab events (such as a start of frame event or at every line of the image).
2. If necessary, specify the trigger signal transition upon which to store the information, using [`M_DATA_LATCH_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md) + [`M_LATCHn`](../../Reference/dig/MdigGetHookInfo.md). Note that this is only useful when [`M_DATA_LATCH_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) + [`M_LATCHn`](../../Reference/dig/MdigGetHookInfo.md) is set to [`M_AUX_IOn`](../../Reference/dig/MdigControl.md)or [`M_TIMER_ACTIVE`](../../Reference/dig/MdigControl.md).
3. Specify the type of information to store, using [`M_DATA_LATCH_TYPE`](../../Reference/dig/MdigControl.md) + [`M_LATCHn`](../../Reference/dig/MdigGetHookInfo.md). Note that a data latch can store only one type of information, and each type of information can only be associated with one data latch, except for timestamp, which can be associated with up to four data latches. Aurora Imaging Library returns an error when this limitation is not respected.
4. To latch data before the start of the first grabbed frame, set[`M_DATA_LATCH_MODE`](../../Reference/dig/MdigControl.md)+ [`M_LATCHn`](../../Reference/dig/MdigGetHookInfo.md) to [`M_PREFETCH`](../../Reference/dig/MdigControl.md). Otherwise, if the data latch is triggered before the start of the first grabbed frame, no information is stored. To latch data only once the grab has started (that is, as of the start of the first grabbed frame), set[`M_DATA_LATCH_MODE`](../../Reference/dig/MdigControl.md)to [`M_DEFAULT`](../../Reference/dig/MdigControl.md); this is the default value.
5. Enable the data latch using [`M_DATA_LATCH_STATE`](../../Reference/dig/MdigControl.md) + [`M_LATCHn`](../../Reference/dig/MdigGetHookInfo.md) set to [`M_ENABLE`](../../Reference/dig/MdigControl.md).
6. If necessary, repeat steps 1 to 5 for each data latch used.
7. Perform the grab. You can use[`MdigProcess`](../../Reference/dig/MdigProcess.md) to grab an image; in the hook-handler function, retrieve and make use of the information stored in the data latch, using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_DATA_LATCH_VALUE`](../../Reference/dig/MdigGetHookInfo.md). Alternatively, hook a hook-handler function to the end of grab frame event ([`M_GRAB_FRAME_END`](../../Reference/dig/MdigControl.md)) using [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md), and call [`MdigGrab`](../../Reference/dig/MdigGrab.md) to grab an image; then, to retrieve and make use of the information stored in the data latch, call [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_DATA_LATCH_VALUE`](../../Reference/dig/MdigGetHookInfo.md).
8. When done, disable the data latches using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_DATA_LATCH_STATE`](../../Reference/dig/MdigControl.md) + [`M_LATCHn`](../../Reference/dig/MdigGetHookInfo.md) set to [`M_DISABLE`](../../Reference/dig/MdigControl.md).

## Triggering a data latch

Data latches can store data at several different points during a grab. Below are a few events that can trigger a data latch.

*[Image: radient_data_latches.png]*

|   |   |   |
| --- | --- | --- |
| Callout # | [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_DATA_LATCH_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) | [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_DATA_LATCH_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md) |
| 1, 4, and 8 | [`M_AUX_IOn`](../../Reference/dig/MdigControl.md) | [`M_EDGE_RISING`](../../Reference/dig/MdigControl.md) |
| 2, 5, and 9 | [`M_TIMER_ACTIVE`](../../Reference/dig/MdigControl.md) | [`M_EDGE_RISING`](../../Reference/dig/MdigControl.md) or [`M_EDGE_FALLING`](../../Reference/dig/MdigControl.md) |
| 3 and 7 | [`M_GRAB_FRAME_START`](../../Reference/dig/MdigControl.md) |
| 6 and 10 | [`M_GRAB_FRAME_END`](../../Reference/dig/MdigControl.md) |
| 3 to 6 and 7 to 10 | [`M_GRAB_LINE`](../../Reference/dig/MdigControl.md) |

Events that occur before the start of the first grabbed frame of queued grabs or a grab sequence (such as, any auxiliary I/O change event, rotary decoder position change, or timer active event) can only trigger a data latch if the data latch has its mode set to prefetch (using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_DATA_LATCH_MODE`](../../Reference/dig/MdigControl.md) set to [`M_PREFETCH`](../../Reference/dig/MdigControl.md)).

In the above image, if the data latch mode is set to prefetch (using [`M_DATA_LATCH_MODE`](../../Reference/dig/MdigControl.md)set to [`M_PREFETCH`](../../Reference/dig/MdigControl.md)), reading the data latch at the end of frame 1 could return the information latched at captions 1 through 6, depending on which event triggers the data latch. However, if the data latch mode is set to default ([`M_DATA_LATCH_MODE`](../../Reference/dig/MdigControl.md)to [`M_DEFAULT`](../../Reference/dig/MdigControl.md)), reading the data latch at the end of frame 1 could only return captions 3 through 6. In both cases, reading the data latch at the end of frame 2 could return captions 7 though 10. Captions 5 and 6 are returned at the end of frame 1, even though they relate to frame 2 because the data of all enabled data latches is returned at the end of every grabbed frame end event (that is, at the end of each grabbed frame).

If calls to [`MdigGrab`](../../Reference/dig/MdigGrab.md)occur while no other grab is currently being performed and [`M_DATA_LATCH_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_PREFETCH`](../../Reference/dig/MdigControl.md), events that occur from the moment the grab command is issued until the start of the grabbed frame can also trigger a data latch and be retrieved at the end of the grabbed frame, for each [`MdigGrab`](../../Reference/dig/MdigGrab.md) call.

*[Image: radient_data_latches_monoshot.png]*

|   |   |   |
| --- | --- | --- |
| Callout # | [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_DATA_LATCH_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) | [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_DATA_LATCH_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md) |
| A and F | [`M_AUX_IOn`](../../Reference/dig/MdigControl.md) | [`M_EDGE_RISING`](../../Reference/dig/MdigControl.md) |
| B and G | [`M_TIMER_ACTIVE`](../../Reference/dig/MdigControl.md) | [`M_EDGE_RISING`](../../Reference/dig/MdigControl.md) or [`M_EDGE_FALLING`](../../Reference/dig/MdigControl.md) |
| C and H | [`M_GRAB_FRAME_START`](../../Reference/dig/MdigControl.md) |
| D and I | [`M_GRAB_FRAME_END`](../../Reference/dig/MdigControl.md) |
| C to D and H to I | [`M_GRAB_LINE`](../../Reference/dig/MdigControl.md) |
| E | No data can be latched during this period |

In the above image, if the data latch mode is set to [`M_PREFETCH`](../../Reference/dig/MdigControl.md), reading the data latch at the end of frame 1 could return the information latched at captions A through D, depending on which event triggers the data latch. In this example, no data latch can return information relating to events during caption E. Frame 2 could return the information latched at captions F through I. Note that, when using [`MdigProcess`](../../Reference/dig/MdigProcess.md) or when queuing grabs using [`MdigGrab`](../../Reference/dig/MdigGrab.md) in asynchronous mode, there is no dead zone.

## Data latch limitations

To set the type of data that a data latch should store, use [`M_DATA_LATCH_TYPE`](../../Reference/dig/MdigControl.md). In most cases, you can set up only one data latch to record a specific type of data (for example, only one data latch can store the status of all I/O signals). The exceptions to this are data latches that store timestamps; you can set up four data latches to store timestamps. Aurora Imaging Library returns an error when this limitation is not respected.

## Retrieving data latch information

Aurora Imaging Library allows each data latch to store multiple instances of the same type of data. You can retrieve a specific instance using the combination value [`M_VALUE_INDEX()`](../../Reference/dig/MdigGetHookInfo.md). For example, when latching the timestamp at the end of each grabbed line of a frame with 1024 lines, to retrieve the 1024th timestamp, use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_DATA_LATCH_VALUE`](../../Reference/dig/MdigGetHookInfo.md) + [`M_LATCHn`](../../Reference/dig/MdigGetHookInfo.md)+ [`M_VALUE_INDEX(1023)`](../../Reference/dig/MdigGetHookInfo.md), where _n_ is the number associated with the data latch.

To retrieve all the stored information associated with a specific data latch, use [`M_DATA_LATCH_VALUE_ALL`](../../Reference/dig/MdigGetHookInfo.md) + [`M_LATCHn`](../../Reference/dig/MdigGetHookInfo.md). Using the previous example, this would return 1024 timestamps.

To inquire how many pieces of information are stored inside a data latch, use [`M_DATA_LATCH_VALUE_COUNT`](../../Reference/dig/MdigGetHookInfo.md).

To convert a timestamp from clock ticks to seconds, use the following equation: `_Timestamp_ * (`_TimestampFrequencyInHz_). To inquire the clock frequency, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_DATA_LATCH_CLOCK_FREQUENCY`](../../Reference/dig/MdigInquire.md).

## Data latch example

For an example of how to use the Zebra Rapixo CoF family data latches, refer to the _DataLatch.cpp_ example.
