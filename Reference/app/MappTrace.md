---
doctype: Reference
module: app
function: MappTrace
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / app / MappTrace"
---

# MappTrace

> Create a user trace marker or section in a trace log.

## Syntax

```c
void MappTrace(
    AIL_ID             ContextAppId,  //out
    AIL_INT64          TraceType,     //in
    AIL_INT64          TraceTag,      //in
    AIL_INT64          TraceValue,    //in
    AIL_CONST_TEXT_PTR TraceString    //in
)
```

## Description

This function creates an Aurora Imaging Library trace marker or section that allows runtime events to be easily identified in a trace log. Add a trace marker or section at places in your code that need attention, such as before entering or after exiting a block of code or before and after an external event. You can create a trace log using Aurora Imaging Profiler, or by calling [`MappControl`](../../Reference/app/MappControl.md) with [`M_TRACE`](../../Reference/app/MappControl.md) set to [`M_LOG_ENABLE`](../../Reference/app/MappControl.md). See [Profiler](../../UserGuide/C65_Development_and_Debugging_Tools_and_Techniques/S01_Profiler.md) for more information on creating a trace log.

When creating a trace marker, only a single function call to [`MappTrace`](../../Reference/app/MappTrace.md) is needed ([`TraceType`](../../Reference/app/MappTrace.md) set to [`M_TRACE_MARKER`](../../Reference/app/MappTrace.md)). When creating a trace section, two calls to [`MappTrace`](../../Reference/app/MappTrace.md) are needed, one function call to start the section and another call to end the section ([`TraceType`](../../Reference/app/MappTrace.md) set to [`M_TRACE_SECTION_START`](../../Reference/app/MappTrace.md) and [`M_TRACE_SECTION_END`](../../Reference/app/MappTrace.md), respectively).

When creating a trace marker or section, you can assign it a trace tag ([`TraceTag`](../../Reference/app/MappTrace.md)), a trace value ([`TraceValue`](../../Reference/app/MappTrace.md)), and/or a trace string ([`TraceString`](../../Reference/app/MappTrace.md)). The trace tag is a fixed user-defined value that identifies the trace marker or section. The trace value and trace string are used to display information relevant to the current placement of the marker; typically, these are variables that depend on runtime events, such as the calculated or return value of a user-defined function. When reading a trace log with Aurora Imaging Profiler, you can directly search for your trace markers or sections and display the relevant information. Note that when creating a trace section, both calls to [`MappTrace`](../../Reference/app/MappTrace.md) must have the same [`TraceTag`](../../Reference/app/MappTrace.md) value.

Optionally, this function can set additional trace tag information using [`TraceType`](../../Reference/app/MappTrace.md) set to [`M_TRACE_SET_TAG_INFORMATION`](../../Reference/app/MappTrace.md). Once this additional tag information is set, all trace markers or sections that have the same [`TraceTag`](../../Reference/app/MappTrace.md) value will use the trace tag's color and name. When viewing an  _.mtrace_ file in Aurora Imaging Profiler, trace markers and sections display this new tag information, along with the original trace values and trace strings assigned when the trace marker or section was created. Together, these pieces of information add an extra level of convenience when searching for trace markers or sections.

> **Note:** Note that [`MappTrace`](../../Reference/app/MappTrace.md) does nothing if you are not generating a trace log.

## Parameters

### `ContextAppId` *(out, AIL_ID)*

Specifies the identifier of the application context to use.

*For specifying the application context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application context identifier` | Specifies the identifier of an application context.

Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappTrace.md). |

### `TraceType` *(in, AIL_INT64)*

Specifies the type of trace marker to create.

*For specifying the trace type*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_TRACE_MARKER` *(default)* | Specifies to create a trace marker that identifies a single point in the code. |
| `M_TRACE_SECTION_END` | Specifies to end a user trace section. It must come after [`M_TRACE_SECTION_START`](../../Reference/app/MappTrace.md), and must have the same [`TraceTag`](../../Reference/app/MappTrace.md) value as the corresponding start. |
| `M_TRACE_SECTION_START` | Specifies to start a user trace section. It must be followed by a call to [`MappTrace`](../../Reference/app/MappTrace.md) with [`M_TRACE_SECTION_END`](../../Reference/app/MappTrace.md). |
| `M_TRACE_SET_TAG_INFORMATION` | Specifies to associate a name and/or color with the specified trace tag. Trace markers or sections that have the same tag will display this color an/or name in Aurora Imaging Profiler.

Associating a color allows the user to quickly identify different trace markers or sections instead of seeing the same color for all user traces in Aurora Imaging Profiler. Associating a name to a marker or section can be helpful in searching for the marker's or section's name rather than its numerical [`TraceTag`](../../Reference/app/MappTrace.md) value.

> **Note:** Note that when generating a trace log in memory, the color and name associated with a trace tag might not be displayed, dependent on if the trace element associated with this call is still in memory or not. |

### `TraceTag` *(in, AIL_INT64)*

Specifies a value that identifies this trace marker or section.

*For specifying the trace tag*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no trace tag is used for this trace marker or section. |
| `1 <= Value <= 255` | Specifies a user-defined value. When using Aurora Imaging Profiler, you can search for a marker or section based on its [`TraceTag`](../../Reference/app/MappTrace.md) value.

When creating a trace section, both the start function call and the end function call must specify the same [`TraceTag`](../../Reference/app/MappTrace.md) value.

When [`TraceType`](../../Reference/app/MappTrace.md) is set to [`M_TRACE_SET_TAG_INFORMATION`](../../Reference/app/MappTrace.md), this value specifies the trace tag to apply the tag information to. All trace markers and sections with the same trace tag display the tag's color and/or name. |

### `TraceValue` *(in, AIL_INT64)*

Specifies a value relevant to the trace marker or section, or relevant to the surrounding code.

*For specifying the trace value*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no trace value is logged for this trace marker or section. |
| `Value` | Specifies a user-defined value. |

*For specifying the trace color*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default color. |
| `M_RGB888` | Specifies an RGB value used to clear an 8-bit 3-band buffer to an RGB color. Each value (red, green or blue) must not exceed 8 bits. |

### `TraceString` *(in, AIL_CONST_TEXT_PTR)*

Specifies a string relevant to the trace marker or section, or relevant to the surrounding code.

*For specifying the trace string*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no trace string is logged for this trace marker or section. |
| `"TraceString"` | Specifies a user-defined string. |

*For specifying the trace name*

| Value | Description |
| --- | --- |
| `"M_DEFAULT"` | Specifies the default name, which is equivalent to the value of the [`TraceTag`](../../Reference/app/MappTrace.md) parameter. |
| `"TraceTagName"` | Specifies a user-defined name for the trace tag. |
