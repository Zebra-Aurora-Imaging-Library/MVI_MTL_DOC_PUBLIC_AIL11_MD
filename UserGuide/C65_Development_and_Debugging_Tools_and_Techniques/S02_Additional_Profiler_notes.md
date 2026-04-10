---
doctype: UserGuide
part: "Miscellaneous"
chapter: Development_and_Debugging_Tools_and_Techniques
section: Additional_Profiler_notes
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / debugging_tools / Additional Profiler notes"
---

# Miscellaneous user guide information for Aurora Imaging Profiler

This section includes additional user guide information for Aurora Imaging Profiler. The information found in this section might be a reiteration of content previously documented.

## Memory buffers

Aurora Imaging Profiler supports trace generation in a circular memory buffer. The buffer can be located in shared memory or in non-paged memory (if available). You can adjust the size of this buffer from the trace generation dialog. You can add multiple buffers to the trace generation dialog to allow tracing multiple applications. Aurora Imaging Profiler can load and display the current snapshot of the buffer while the application is tracing.

## New functionalities

The following improvements are now available:

- Double-clicking on an event in a list in the Profiler now zooms on the event in addition to re-centering on the event.
- Aurora Imaging Profiler can now search for memory traces inside memory dump files (.dmp).
- The folder used by Aurora Imaging Library to generate trace files has been replaced by a folder with less access restriction, allowing services running under a different user to generate traces.
- The trace loading dialog now supports file deletion.
- The statistics pane now supports exporting statistic results.

## Statistics

Aurora Imaging Profiler now provides statistical information relative to the execution times of operations. The statistics tab in the right pane provides information such as the minimum, maximum, total, and average execution time. You can select and highlight operations in the timeline and find corresponding statistical information in the statistics tab.

## Constants conversion

Aurora Imaging Profiler now converts the numerical values of function parameters into their definition names. This feature can be deactivated from the Options dialog. To open this dialog, choose Options from the **Edit** menu item.

## Improved trace loading

When opening a trace, The **Open Trace** dialog allows you to modify the section of the file to load. By default, the size of the section is automatically adjusted to accommodate the available memory. To open this dialog, choose Open Trace... or Load Other Section from the **File** menu item.

Trace generation now displays a window that lists the currently available trace files. This list is updated automatically as traces are created.

## AIL_ID information

AIL_IDs are now displayed as hyperlinks in the trace detail section at the bottom of the main Profiler window. When clicked, a popup containing the most probable allocation and free operations for the ID is displayed. From this popup, you can also jump to the previous or next occurrence of the ID.

## Bookmarks

Markers are now called Bookmarks and are listed in a separate tab in the Quick Access sidebar. You can add measuring points directly to a bookmark. To do so, open the bookmark context menu by right-clicking on the bookmark in the Bookmarks tab and select Set First Measuring Point or Set Second Measuring Point. From the context menu, you can also rename a bookmark or delete it.

## Miscellaneous features

The following features are also available:

- Holding the Shift key while zooming with the mouse wheel zooms faster.
- Search results have a contextual menu. To display search results, click on the Find button (binoculars) or press Ctrl + F to open the Find dialog. Then, specify the required information and click Find.
- Initially loaded DLLs are shown in the Trace Information window even with a partially loaded trace.
- Used license packages are displayed in the Trace Information window if [`MappFree`](../../Reference/app/MappFree.md) is contained in the loaded section of the trace.
- The trace overview section, shown at the bottom of the Profiler window, supports more intuitive mouse controls. For example, Ctrl+Click draws a region, and you can resize the viewport region by grabbing and moving the region's edges.
- You can resize a thread by dragging the gripper at the bottom of the thread header.
- You can display Timestamps in seconds instead of milliseconds.
- Aurora Imaging Profiler settings can be reset to default from the new Options dialog box. To open this dialog, choose Options from the **Edit** menu item.
- Aurora Imaging Profiler supports Aurora Design Assistant traces. These traces include information about features such as loops, steps, flowcharts, filmstrips, operator views, and errors.

Also note the following:

- Combining application and thread level activation or deactivation of the trace log can lead to unexpected results when visualized with Aurora Imaging Profiler.
- Some numerical values of the function parameters are converted into definition names that might be inaccurate.
