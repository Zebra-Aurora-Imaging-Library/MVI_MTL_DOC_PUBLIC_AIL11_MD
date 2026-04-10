---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Error_reporting
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Error reporting"
---

# Error reporting

You can enable or disable error reporting to the Host screen, using [`MappControl`](../../Reference/app/MappControl.md). By default, error reporting is enabled. If you disable error reporting, you can still determine the success of a particular function or a sequence of functions, using [`MappGetError`](../../Reference/app/MappGetError.md). In addition, you can assign a user-defined function to handle the event of an Aurora Imaging Library error using [`MappHookFunction`](../../Reference/app/MappHookFunction.md).

For an example that uses [`MappGetError`](../../Reference/app/MappGetError.md) to determine if the allocation of the default application, default system, default display, and default image buffer is a success, see _MAppStart.cpp_ in [Installation and before you start](S02_Installation.md).

For assistance with debugging your application, see [Aurora Imaging Profiler and trace logs](../C65_Development_and_Debugging_Tools_and_Techniques/S01_Profiler.md).
