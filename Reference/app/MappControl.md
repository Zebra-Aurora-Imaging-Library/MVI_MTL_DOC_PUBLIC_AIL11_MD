---
doctype: Reference
module: app
function: MappControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / app / MappControl"
---

# MappControl

> Control an application environment setting.

## Syntax

```c
void MappControl(
    AIL_ID    ContextAppId,  //out
    AIL_INT64 ControlType,   //in
    AIL_INT   ControlValue   //in
)
```

## Description

This function controls the settings of an Aurora Imaging Library application environment. These settings include printing error messages to the screen, parameter checking, logging trace information, as well as on-board memory and processing compensation modes. This function can also clear current and/or global errors.

In multi-thread environments, an [`MappControl`](../../Reference/app/MappControl.md) call applies to all application threads running Aurora Imaging Library, unless otherwise stated (or specifically limited to the calling thread by adding [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) to the [`ControlType`](../../Reference/app/MappControl.md) parameter). When you override settings for a specific thread, a subsequent call to change those settings from any other thread will not change the settings for the current thread. For example, if you enable trace printing for a particular thread, that thread will ignore calls from other threads that try to change trace printing setting.

## Parameters

### `ContextAppId` *(out, AIL_ID)*

Specifies the identifier of the application context to use.

*For specifying the application context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application context identifier` | Specifies the identifier of an application context.

Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappControl.md). |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of application environment setting to control.

### `ControlValue` *(in, AIL_INT)*

Specifies the setting's new value.

## Parameter Associations

### For managing error functionality

The following [`ControlType`](../../Reference/app/MappControl.md) and corresponding [`ControlValue`](../../Reference/app/MappControl.md) parameter settings are used to manage error functionality in the application.

---

### `M_CLEAR_ERROR`

Clears current and/or global errors.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to clear both current and global errors. |
| `M_CURRENT` | Specifies to clear only current errors. |
| `M_GLOBAL` | Specifies to clear only global errors. |

---

### `M_ERROR`

Sets whether the printing of error messages to screen is enabled.  > **Note:** Note that, within a user-defined function, this setting is independent of the main application. If you enable or disable printing of error messages within a callee function, that setting will not be retained when the callee function ends.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_PRINT_ENABLE`](../../Reference/app/MappControl.md), unless [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) is specified, in which case this setting specifies that the current thread uses the setting of the application environment. |
| `M_PRINT_DISABLE` | Specifies not to print error messages. If error printing is disabled, you can still check for error, using [`MappGetError`](../../Reference/app/MappGetError.md). |
| `M_PRINT_ENABLE` | Specifies to print error messages. |
| `M_THROW_EXCEPTION` | Specifies that when an error occurs, a .NET or Python exception will be thrown instead of a message box being displayed.  > **Note:** [`M_THROW_EXCEPTION`](../../Reference/app/MappControl.md) is only valid when working in a  .NET or Python environment. |

---

### `M_ERROR_HOOKS`

Sets whether errors generated in the current thread will trigger the user-defined function that you specified using [`MappHookFunction`](../../Reference/app/MappHookFunction.md) (with[`M_ERROR_CURRENT`](../../Reference/app/MappHookFunction.md) or [`M_ERROR_FATAL`](../../Reference/app/MappHookFunction.md)).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies to not trigger the user-defined function (hooked using [`MappHookFunction`](../../Reference/app/MappHookFunction.md)) when an error event occurs.  This ignores the functionality of [`MappHookFunction`](../../Reference/app/MappHookFunction.md) with regards to error event types, until the functionality is restored using [`M_ERROR_HOOKS`](../../Reference/app/MappControl.md) set to [`M_ENABLE`](../../Reference/app/MappControl.md).  > **Note:** If an error of the appropriate type is generated in another thread, the user-defined function will still be triggered. |
| `M_ENABLE` *(default)* | Specifies to trigger the user-defined function when an error event occurs, as specified in [`MappHookFunction`](../../Reference/app/MappHookFunction.md). |

### For specifying general application environment settings

The following [`ControlType`](../../Reference/app/MappControl.md) and corresponding [`ControlValue`](../../Reference/app/MappControl.md) parameter settings are used to control the application environment.

---

### `M_COMPONENT_AUTO_RESELECT`

Sets whether image buffer components are automatically reselected to 2D displays when they are reallocated. A component is considered reallocated when a single function internally frees the component and allocates a new similar component in the same container.  > **Note:** This setting does not affect containers shown in 3D displays.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_COMPONENT_AUTO_RESELECT_ENABLE`](../../Reference/app/MappControl.md), unless [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) was specified, in which case this setting specifies that the current thread uses the setting of the application environment. |
| `M_COMPONENT_AUTO_RESELECT_DISABLE` | Specifies not to automatically reselect image buffer components to 2D displays. If a component selected to a 2D display is reallocated, the 2D display will close. |
| `M_COMPONENT_AUTO_RESELECT_ENABLE` *(default)* | Specifies to automatically reselect image buffer components to 2D displays when they are reallocated.  For example, this is useful if you want to display a single component from a container that you are grabbing into and the layout of the container might change between grabs (in which case, the components of the container are freed and reallocated). |

---

### `M_DAIL_CONNECTION`

Specifies the Distributed Aurora Imaging Library monitoring configuration global permission level.  This allows or disallows another application, the monitoring application, to provisionally access specified objects in the current application, the publishing application. Each object in the publishing application must individually allow either read only, or read/write access by calling [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_DAIL_PUBLISH`](../../Reference/obj/MobjControl.md) set to either [`M_READ_ONLY`](../../Reference/obj/MobjControl.md) or [`M_READ_WRITE`](../../Reference/obj/MobjControl.md), respectively. Any object not specified as read only or read/write is not accessible by the monitoring application.

| Value | Description |
| --- | --- |
| `M_DAIL_CONTROL` | Specifies that objects in the current application can be accessed with either read only or read/write permission, depending on how that object was specified with [`MobjControl`](../../Reference/obj/MobjControl.md). |
| `M_DAIL_MONITOR` | Specifies that objects in the current application can be accessed only with read only permission, regardless of how that object was specified with [`MobjControl`](../../Reference/obj/MobjControl.md). |
| `M_DISABLE` | Specifies that Distributed Aurora Imaging Library monitoring configuration is disabled in the current application. Once disabled, a monitoring application cannot access any objects in the current application. |

---

### `M_DAIL_CONNECTION_PORT`

Specifies the listening port for a publishing application in the Distributed Aurora Imaging Library monitoring configuration. If the publishing application has specified a listening port, the monitoring application connecting to the publishing application must specify that listening port. If the publishing application does not specify a listening port, the default listening port, set using the Aurora Imaging Configurator utility, is used.  [`M_DAIL_CONNECTION_PORT`](../../Reference/app/MappControl.md) only applies when the current application is a publishing application in a Distributed Aurora Imaging Library monitoring configuration.

| Value | Description |
| --- | --- |
| `Listening port` | Specifies the listening port (an integer between 0 and 65535) that the current application will use to listen for connections from a monitoring application. |

---

### `M_MEMORY`

Sets whether on-board memory compensation on the Host is enabled. On-board memory compensation allows memory to be allocated on Host when a system has insufficient memory to perform that memory allocation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_COMPENSATION_ENABLE`](../../Reference/app/MappControl.md), unless [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) is specified, in which case this setting specifies that the current thread uses the setting of the application environment. |
| `M_COMPENSATION_DISABLE` | Specifies that on-board memory compensation is disabled. If there is insufficient memory available for the allocation, an error is generated. |
| `M_COMPENSATION_ENABLE` | Specifies that on-board memory compensation is enabled. |

---

### `M_PARAMETER`

Sets whether the checking of parameters is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_CHECK_ENABLE`](../../Reference/app/MappControl.md), unless [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) is specified, in which case this setting specifies that the current thread uses the setting of the application environment. |
| `M_CHECK_DISABLE` | Specifies that checking of parameters is disabled. Note, if parameter checking is disabled (for example, to accelerate an application), unpredictable behavior can be expected when passing invalid parameters to a function. |
| `M_CHECK_ENABLE` | Specifies that checking of parameters is enabled. |

---

### `M_PROCESSING`

Sets whether the Host should compensate for processes that cannot be performed on-board due to board-specific limitations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_COMPENSATION_ENABLE`](../../Reference/app/MappControl.md), unless [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) is specified, in which case this setting specifies that the current thread uses the setting of the application environment. |
| `M_COMPENSATION_DISABLE` | Specifies that on-board processing compensation is disabled. If an operation cannot be performed by the system's on-board processor, an error is generated. |
| `M_COMPENSATION_ENABLE` | Specifies that on-board processing compensation is enabled. If an operation cannot be performed by the system's on-board processor, it will be performed by the Host. |

---

### `M_TRACE`

Sets the trace setting, which controls when a trace log is generated.  The trace log is a binary file with a _.mtrace_ extension and is readable only with Aurora Imaging Profiler.  This control type is synchronous.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default trace setting, which either enables or disables generating a trace log, based on whether Aurora Imaging Profiler (or the Interactive log) was used to start generating the log.  If Aurora Imaging Profiler (or the Interactive log) started generating the trace log, this control value is the same as [`M_LOG_ENABLE`](../../Reference/app/MappControl.md).  If a function started generating the trace log, this control value is the same as [`M_LOG_DISABLE`](../../Reference/app/MappControl.md). |
| `M_LOG_DISABLE` | Specifies the log disable setting, which stops generating the trace log. |
| `M_LOG_ENABLE` | Specifies the log enable setting, which starts generating the trace log. |

---

### `M_TRACE_SAVE_TO_FILE`

Transfers gathered trace data to file depending on how the trace log is generated.  When generating a trace log to memory with Aurora Imaging Profiler, this control type transfers all gathered trace data currently in the temporary circular trace log buffer in chronological order to an  _.mtrace_ file with the specified path and/or file name. The resulting  _.mtrace_ file only contains the most recent trace data present in the circular buffer.  When generating a trace log to file, this control type forces all outstanding writes to be performed to the  _.mtrace_ file in the default file path. When generating the trace log using Aurora Imaging Profiler, the default file path is_%PROGRAMDATA%\Aurora Imaging Library\11\Profiler_; otherwise, the default file path is %tmp%. The file will contain all trace data from the moment the trace started gathering data until the moment when this control is called. In this case, [`ControlValue`](../../Reference/app/MappControl.md)must be set to [`M_NULL`](../../Reference/app/MappControl.md).

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to use the default path and default file name. |
| `"FileName"` | Specifies the file name, but uses the default path. For instance, AIL_TEXT("Test.mtrace"). |
| `"Path"` | Specifies the path, but uses the default file name. For instance, AIL_TEXT("D:\\MyFolder").  > **Note:** Note that every folder in the path must already exist. This control only uses existing folders. |
| `"PathAndFileName"` | Specifies the path and the file name. For instance, AIL_TEXT("D:\\MyFolder\\Test.mtrace").  > **Note:** Note that every folder in the path must already exist. This control only uses existing folders. |

---

### `M_WEB_CONNECTION`

Sets whether Aurora Imaging Web Library server functionality is enabled.  When Aurora Imaging Web Library server functionality is enabled, your application is a server application. You can publish some types of objects using [`MobjControl`](../../Reference/obj/MobjControl.md)with [`M_WEB_PUBLISH`](../../Reference/obj/MobjControl.md). You can access these objects over the network using an Aurora Imaging Web Library client application (for example, running in a web browser).  For more information about Aurora Imaging Web Library, including how to create a client application, see[Using Aurora Imaging Web Library to monitor your application](../../UserGuide/C58_Using_AIWL_to_monitor_your_application/ChapterInformation.md).  > **Note:** Aurora Imaging Web Library servers are intended to be run on a segmented LAN that has no internet/WAN access; servers offer no internal security mechanisms. When Aurora Imaging Web Library is enabled, you should typically ensure that your network administrator configures your network to prevent incoming connections from the internet/WAN to the computer running your application (and from any other devices on your network that do not need access). If this is not possible, at a minimum you should block incoming connections from the internet to the listening port of the server. You can inquire which port is used for this purpose, using [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_WEB_CONNECTION_PORT`](../../Reference/app/MappInquire.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that Aurora Imaging Web Library server functionality is disabled in the current application. |
| `M_ENABLE` | Specifies that Aurora Imaging Web Library server functionality is enabled in the current application. |

---

### `M_WEB_CONNECTION_PORT`

Sets the listening port used for Aurora Imaging Web Library server functionality. If you specify a listening other than the default, your Aurora Imaging Web Library client application must specify that port when establishing a connection. If the server application does not specify a listening port, the default listening port 7861 is used.  > **Note:** For increased security, you should ensure that remote computers cannot access this port from the internet/WAN.

| Value | Description |
| --- | --- |
| `0 <= Value <=65535` *(default)* | Specifies the port number.  > **Note:** This port must not be used by any other application (including third-party applications) running on the local computer. |

### Combination Constants — For limiting effects to the current thread

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set whether the control should be limited to the current thread.

| Value | Description |
| --- | --- |
| `M_THREAD_CURRENT` | Limits the effect of the control type setting to the current thread.  > **Note:** Note that when this value is added to [`M_TRACE`](../../Reference/app/MappControl.md), [`M_TRACE`](../../Reference/app/MappControl.md) set to [`M_DEFAULT`](../../Reference/app/MappControl.md) will adopt the application's current global trace state (enabled or disabled). |
