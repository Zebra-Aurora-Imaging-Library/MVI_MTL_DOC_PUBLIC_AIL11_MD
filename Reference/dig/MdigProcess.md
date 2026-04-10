---
doctype: Reference
module: dig
function: MdigProcess
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dig / MdigProcess"
---

# MdigProcess

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Grab frames of data sequentially into a list of manually or automatically allocated image buffers or containers, and process them with a user-defined function as they are grabbed.

## Syntax

```c
void MdigProcess(
    AIL_ID                    DigId,                            //in
    const AIL_ID *            DestContainerOrImageBufArrayPtr,  //in
    AIL_INT                   ImageCount,                       //in
    AIL_INT64                 Operation,                        //in
    AIL_INT64                 OperationFlag,                    //in
    AIL_DIG_HOOK_FUNCTION_PTR HookHandlerPtr,                   //in
    void *                    UserDataPtr                       //in-out
)
```

## Description

This function uses the specified digitizer to acquire frames of data and store them sequentially in a list of image buffers or a list of containers. It also hooks a user-defined function to the modification of any buffers/containers in the specified list. The user-defined function is therefore called every time a new frame is grabbed.

Optionally, instead of grabbing data into a list of previously allocated buffers or containers, this function can automatically allocate an appropriate image buffer or container to store each grabbed frame of data. This is particularly useful when grabbing from a camera that is configured to transmit images with different dimensions for each grab. To specify that the buffers/containers should be automatically allocated, set [`DestContainerOrImageBufArrayPtr`](../../Reference/dig/MdigProcess.md)to[`M_NULL`](../../Reference/dig/MdigProcess.md).

> **Note:** When explicitly passing an array of image buffer or containers, note the following. When grabbing an image, you should use an image buffer as a destination. When grabbing any other type of data (such as 3D data), you should use a container as a destination. You can determine what type of Aurora Imaging Library object the digitizer is designed to grab into using [`MdigInquire`](../../Reference/dig/MdigInquire.md)with [`M_TARGET_BUFFER_OBJECT`](../../Reference/dig/MdigInquire.md).

If set synchronously, [`MdigProcess`](../../Reference/dig/MdigProcess.md) will block the thread until the list of sequential grabs has completed. If set asynchronously, the buffers/containers are prepared, and then control is returned to the calling thread while, in the background, the grabs will begin.

The grabs can stop at the end of the list, after a predefined number of grabs occur, or when a stop instruction is explicitly given. In the latter two cases, grabbing occurs in a round robin fashion; buffers/containers are filled with the grabbed data typically in the order they are stored in the list, wrapping around to the first buffer/container in the list once the last buffer/container in the list is filled.

The index of the modified buffer/container can be inquired using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_MODIFIED_BUFFER`](../../Reference/dig/MdigGetHookInfo.md)+[`M_BUFFER_INDEX`](../../Reference/dig/MdigGetHookInfo.md). The Aurora Imaging Library identifier of the modified buffer/container can be inquired using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_MODIFIED_BUFFER`](../../Reference/dig/MdigGetHookInfo.md)+[`M_BUFFER_ID`](../../Reference/dig/MdigGetHookInfo.md). To identify whether the modified buffer/container (that is, the current frame) is corrupt (for example, because a packet was not received successfully), use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_CORRUPTED_FRAME`](../../Reference/dig/MdigGetHookInfo.md).

Once a frame of data is grabbed into a buffer/container, Aurora Imaging Library will not grab into that buffer/container again until the hook function has finished processing it. Once the hook function finishes executing, the buffer/container becomes available for Aurora Imaging Library to grab into it. If all buffers/containers are filled with data and are waiting to be processed by the hook function, Aurora Imaging Library waits for the next available buffer/container (typically the next buffer/container in the list) before grabbing another frame of data.

> **Note:** You can use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_PROCESS_...`](../../Reference/dig/MdigInquire.md) to inquire the status of process operation. For example, if the average time it takes to process a frame of data is greater than the frame rate of a camera, frames will eventually be missed. To check if this is happening, you can use [`MdigInquire`](../../Reference/dig/MdigInquire.md)with[`M_PROCESS_PENDING_GRAB_NUM`](../../Reference/dig/MdigInquire.md) in the hook function to determine if the expected number of processing buffers are currently available.

To grab until a stop instruction, you must call [`MdigProcess`](../../Reference/dig/MdigProcess.md) with [`M_START`](../../Reference/dig/MdigProcess.md); when you want to stop grabbing, you must call [`MdigProcess`](../../Reference/dig/MdigProcess.md) with [`M_STOP`](../../Reference/dig/MdigProcess.md). Once the stop instruction is issued, no more frames will be grabbed. Buffers/containers that store unprocessed frames are processed before the original calling thread continues, unless the stop instruction is issued by calling [`MdigProcess`](../../Reference/dig/MdigProcess.md) from within the hook-handler function itself (in which case, processing is completed while the original calling thread continues).

By default, buffers/containers are processed sequentially, in the order into which grabbed; a single instance of the hooked user-defined function will run on a single thread, processing one buffer/container at a time. When running on a computer with multiple CPU cores, you can have the grabbed buffers/containers handled by different instances of the user-defined function running on different threads. To do so, use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_MODIFIED_BUFFER_HOOK_MODE`](../../Reference/sys/MsysControl.md) set to [`M_MULTI_THREAD`](../../Reference/sys/MsysControl.md).

> **Note:** If you are grabbing from multiple cameras simultaneously with the hook function executing in single-threaded mode, each instance of[`MdigProcess`](../../Reference/dig/MdigProcess.md) will use its own thread to execute an instance of the hooked function. Additionally, many Aurora Imaging Library functions are transparently multi-threaded. These factors can combine to generate a large number of threads, potentially slowing down your application. Since having the hook function execute in multi-threaded mode can multiply the number of threads generated by the number of physical CPU cores in your computer (or, optionally, the number of threads that you specify with [`M_MULTI_THREAD`](../../Reference/sys/MsysControl.md)), it is often more efficient to have the hook function execute in single-threaded mode. If you are considering multi-threaded mode, you should benchmark your application under real-world conditions before making a final decision.

When processing with the hook function executing in multi-threaded mode, it is possible for buffers/containers to be grabbed and processed in a different order than they are stored in the list because they are processed at different rates. For example, if the list contains 3 buffers and all of them are being processed, it is possible that processing will finish on the second buffer in the list before the first buffer in the list. In this case, Aurora Imaging Library will grab into the second buffer and cue it for processing instead of waiting for the first buffer to become available.

When the specified digitizer is set to perform triggered grabs, the default behavior of the function is to wait for a trigger event before grabbing each frame. To grab all or a specific number of frames per trigger, set the [`OperationFlag`](../../Reference/dig/MdigProcess.md) parameter to [`M_TRIGGER_FOR_FIRST_GRAB`](../../Reference/dig/MdigProcess.md) and use the [`Operation`](../../Reference/dig/MdigProcess.md) parameter to specify how many frames to grab per trigger. To grab continuously after just one trigger, use +**M_FRAMES_PER_TRIGGER()** set to **M_FRAMES_PER_TRIGGER()**.

> **Note:** Note that if the camera is in triggered mode, you should not also use the digitizer in triggered mode.

It is not recommended to change the digitizer settings of the digitizer currently being used when grabs are still pending.

### System specific

| Board(s) | Note |
|---|---|
| Host System | When grabbing from a video or directory of files, once the last image is grabbed, grabbing will restart from the beginning of the video or from the first file in the directory. |

## Parameters

### `DigId` *(in, AIL_ID)*

Specifies the identifier of the digitizer to be used to perform the grabs.

### `DestContainerOrImageBufArrayPtr` *(in, *AIL_ID)*

Specifies the address of the array containing the identifiers of the image buffers or containers in which to place the grabbed frames of data, or specifies to automatically allocate image buffers or containers appropriate for the grabbed data.

*For specifying the Aurora Imaging Library identifiers to grab into*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to automatically allocate appropriate image buffers or containers with [`M_PROC`](../../Reference/buf/MbufAllocColor.md)to store grabbed frames of data. Automatically allocated image buffers always have the correct size, type, and number of bands to store the grabbed data.

> **Note:** You can specify the maximum number of buffers or containers to internally allocate using the [`ImageCount`](../../Reference/dig/MdigProcess.md). You should specify a sufficient number of internal buffers or containers to allow grabbing new frames of data while previously grabbed frames are being processed. Otherwise, frames of data could be missed.

To learn the Aurora Imaging Library identifiers of the image buffers or containers that were grabbed into, use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_MODIFIED_BUFFER`](../../Reference/dig/MdigGetHookInfo.md)+[`M_BUFFER_ID`](../../Reference/dig/MdigGetHookInfo.md)within the hook-handler function.

> **Note:** You cannot use the Aurora Imaging Library identifiers of automatically allocated buffers outside of the hook function, and you should assume that the memory allocated for each buffer or container will be freed immediately after the hook-handler function that processes it returns. |
| `Color image buffer identifier` | Specifies an array containing the identifiers of color destination image buffers with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute. The following pertains to each image buffer.

This image buffer must have been previously allocated, typically using [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md). This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

If the grab buffer is smaller than the digitizer's frame size, the image buffer will be filled and all other data will be lost. If the grab buffer is larger than the digitizer's frame size, the buffer will be filled up to the digitizer's frame size. The rest of the buffer will remain untouched.

> **Note:** If you grab into a buffer from a digitizer that outputs data in a format suitable for a container, the first intensity component in the frame of data will be grabbed. If there is no intensity component, the first component that is not metadata will be grabbed. |
| `Container identifier` | Specifies an array containing the identifiers of containers with an [`M_GRAB`](../../Reference/buf/MbufAllocContainer.md) attribute. The following pertains to each container.

This container must have been previously allocated, typically using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md).

When grabbing into a container for the first time, Aurora Imaging Library will free all existing components and allocate the components required to store the current frame of data. Aurora Imaging Library will typically reuse these automatically allocated components during subsequent grabs, unless you add or remove components from the container (in which case all components will be freed and new components will again be allocated for the grabbed data).

> **Note:** Aurora Imaging Library will not grab into components of the container that you have allocated manually using [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md), [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md), or [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md).

In rare applications, you might configure your camera/3D sensor to transmit components of a different size or format with each grab. If there is any mismatch between the components from the previous grab and those required to store the current frame of data (for example, if the ordering, component type, or size has changed), all components will be automatically freed and new components will be allocated; the components will be assigned new identifiers. You can inquire the new buffer identifiers using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_COMPONENT_ID`](../../Reference/buf/MbufInquireContainer.md).

> **Note:** If you need the buffer identifier of a component, and it is possible that the components might be reallocated, you should inquire the current buffer identifier after each grab. |
| `Monochrome image buffer identifier` | Specifies an array containing the identifiers of monochrome destination image buffers with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute. The following pertains to each image buffer.

This image buffer must have been previously allocated, typically using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md). This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

It is not possible to grab into a color-band child buffer (that is, a child buffer created from one band of a color parent buffer) when the parent buffer is packed.

When grabbing into an image buffer, if the grab destination buffer is smaller than the digitizer's frame size, the image buffer will be filled and all other data will be lost. If the grab destination buffer is larger than the digitizer's frame size, the buffer will be filled up to the digitizer's frame size. The rest of the buffer will remain untouched.

> **Note:** If you grab into a buffer from a digitizer that outputs data in a format suitable for a container, the first intensity component in the frame of data will be grabbed. If there is no intensity component, the first component that is not metadata will be grabbed. |

### `ImageCount` *(in, AIL_INT)*

Specifies the number of buffers or containers in the array, or the number of internal buffers or containers to automatically allocate.

*For specifying the number of buffers or containers in the array*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to automatically allocate the default number of internal buffers or containers (5); alternatively, in C++, if you passed a _std::vector&lt;AIL_ID>_ to the [`DestContainerOrImageBufArrayPtr`](../../Reference/dig/MdigProcess.md), you can use this setting to have Aurora Imaging Library automatically determine the size. In the former case, this setting is only available when the [`DestContainerOrImageBufArrayPtr`](../../Reference/dig/MdigProcess.md) parameter is set to [`M_NULL`](../../Reference/dig/MdigProcess.md). |
| `Value > 0` | Specifies the number of buffers or containers in the array, or the number of internal buffers or containers to automatically allocate. |

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform.

*For specifying the type of operation to perform*

| Value | Description |
| --- | --- |
| `M_SEQUENCE` | Grabs a specific number of frames, storing them sequentially in a list of buffers or containers. Grabbing ends once the specified number of frames have been captured.

When using a digitizer set to perform triggered grabs and the [`OperationFlag`](../../Reference/dig/MdigProcess.md) parameter is set to [`M_TRIGGER_FOR_FIRST_GRAB`](../../Reference/dig/MdigProcess.md), the function will grab the specified number of frames when the first trigger event occurs. |
| `M_START` | Starts grabbing round-robin into the list of buffers or containers; the grabs will continue until stopped (using [`MdigProcess`](../../Reference/dig/MdigProcess.md) with [`M_STOP`](../../Reference/dig/MdigProcess.md)).

When using a digitizer set to perform triggered grabs and the [`OperationFlag`](../../Reference/dig/MdigProcess.md) parameter is set to [`M_TRIGGER_FOR_FIRST_GRAB`](../../Reference/dig/MdigProcess.md), the function will, by default, start grabbing all frames when the first trigger event occurs. |
| `M_STOP` | Stops the grab. By default, it also cancels all grabs that might have been queued. Note that cancelled grabs will not trigger the hooked function, but the events for the previous grabs are still processed.

To stop a grab started in synchronous mode, the [`Operation`](../../Reference/dig/MdigProcess.md) parameter must be set to [`M_STOP`](../../Reference/dig/MdigProcess.md) and the [`MdigProcess`](../../Reference/dig/MdigProcess.md) function called from a separate thread. |

*For*

| Value | Description |
| --- | --- |
| `M_WAIT` | Stops queuing new grabs and waits for all previously queued grabs to finish before returning control to the Host. |

*For*

| Value | Description |
| --- | --- |
| `M_COUNT` | Specifies the number of frames to grab in the sequence.

This combination constant will cause [`MdigProcess`](../../Reference/dig/MdigProcess.md) to stop grabbing when the grab count (_n_) is reached. If the grab count is greater than the buffer count, the grab will be done in round-robin style. By default, the grab count is equal to the number of buffers or containers in the destination image array. |

*For*

| Value | Description |
| --- | --- |
| `M_FRAMES_PER_TRIGGER` | Specifies the number of frames to grab sequentially when a digitizer trigger event occurs.

> **Note:** Note that to use this setting, the [`OperationFlag`](../../Reference/dig/MdigProcess.md) parameter must be set to [`M_TRIGGER_FOR_FIRST_GRAB`](../../Reference/dig/MdigProcess.md) and the digitizer used must be configured to grab upon a trigger. |

### `OperationFlag` *(in, AIL_INT64)*

Specifies more information about the synchronization and triggering of the grab.

*For controlling the order in which processing occurs*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default execution mode.

If the [`Operation`](../../Reference/dig/MdigProcess.md) parameter is set to [`M_START`](../../Reference/dig/MdigProcess.md), the default is the same as [`M_ASYNCHRONOUS`](../../Reference/dig/MdigProcess.md).

If the [`Operation`](../../Reference/dig/MdigProcess.md) parameter is set to [`M_SEQUENCE`](../../Reference/dig/MdigProcess.md), the default is the same as [`M_SYNCHRONOUS`](../../Reference/dig/MdigProcess.md).

If the [`Operation`](../../Reference/dig/MdigProcess.md) parameter is set to [`M_STOP`](../../Reference/dig/MdigProcess.md), the default is the same as [`M_SYNCHRONOUS`](../../Reference/dig/MdigProcess.md) unless [`MdigProcess`](../../Reference/dig/MdigProcess.md) is called from the user-defined function, in which case the default is the same as [`M_ASYNCHRONOUS`](../../Reference/dig/MdigProcess.md). Note that when the [`Operation`](../../Reference/dig/MdigProcess.md) parameter is set to [`M_STOP`](../../Reference/dig/MdigProcess.md), [`M_DEFAULT`](../../Reference/dig/MdigProcess.md) is the only possible setting for the [`OperationFlag`](../../Reference/dig/MdigProcess.md) parameter. |
| `M_ASYNCHRONOUS` | Allows your thread to continue after initiating the start of the grabs, rather than waiting for [`MdigProcess`](../../Reference/dig/MdigProcess.md) to finish.

Note that when you are grabbing and processing a sequence of images asynchronously, you must make an additional call to MdigProcess using [`M_STOP`](../../Reference/dig/MdigProcess.md) to free internally allocated resources. |
| `M_SYNCHRONOUS` | Synchronizes your thread with the end of [`MdigProcess`](../../Reference/dig/MdigProcess.md). |

*For how the camera calibration context is propagated*

| Value | Description |
| --- | --- |
| `M_CALIBRATION_PROPAGATE_AT_EACH_FRAME` | Specifies that the camera calibration context associated with the digitizer is re-associated with the buffer each time the buffer is modified and just before calling the hook-handler function. This is less error-prone than [`M_CALIBRATION_PROPAGATE_ONCE`](../../Reference/dig/MdigProcess.md), but it might slow down the hook-handler function. |
| `M_CALIBRATION_PROPAGATE_OFF` | Specifies to not affect the camera calibration context associated with the modified image buffer. You must associate the required camera calibration context with the image buffers before calling [`MdigProcess`](../../Reference/dig/MdigProcess.md), if necessary. |
| `M_CALIBRATION_PROPAGATE_ONCE` *(default)* | Specifies that before starting image acquisition, the camera calibration context associated with the digitizer is also associated with each image buffer.

> **Note:** Since the camera calibration context associated with the digitizer is only propagated at the beginning of the grab sequence, be cautious when using the hook-handler function to change the camera calibration context settings; the camera calibration context associated with the digitizer will not be re-associated with the modified buffer afterwards. |

*For specifying whether to clear the ROI of the previous grab*

| Value | Description |
| --- | --- |
| `M_REGION_DELETE_AT_EACH_FRAME` | Specifies that any ROI associated with the modified buffer is deleted immediately before calling the user-defined hook function. This is equivalent to calling [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) to clear the ROI of the modified buffer after every grab. This is less error-prone, but might slow down the user-defined hook function's execution slightly. |
| `M_REGION_KEEP` *(default)* | Specifies that any ROI associated with the modified buffer is left as is. |

*For only controlling the grab of the first frame with a trigger*

| Value | Description |
| --- | --- |
| `M_TRIGGER_FOR_FIRST_GRAB` | Specifies that a trigger will only be used to control the grab of the first frame. When there is no preset end to the list of grabs ([`M_START`](../../Reference/dig/MdigProcess.md)), you can also force every _n_ <sup>th</sup> grab to wait for a trigger. To do so, set the [`Operation`](../../Reference/dig/MdigProcess.md) parameter to [`M_START`](../../Reference/dig/MdigProcess.md) + **M_FRAMES_PER_TRIGGER()**.

> **Note:** Note that if the digitizer is not configured for triggered grabs, [`M_TRIGGER_FOR_FIRST_GRAB`](../../Reference/dig/MdigProcess.md) is ignored. Also, when the digitizer is configured for triggered grabs, but [`M_TRIGGER_FOR_FIRST_GRAB`](../../Reference/dig/MdigProcess.md) is not set (the default behavior), the function will only grab each frame when triggered. |

### `HookHandlerPtr` *(in, AIL_DIG_HOOK_FUNCTION_PTR)*

Specifies the address of the function that is called when a buffer/container is modified in the array of destination image buffers/containers.

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/dig/MdigProcess.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if not used.
