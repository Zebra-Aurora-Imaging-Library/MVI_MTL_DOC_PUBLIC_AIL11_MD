---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Target_system
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Target system"
---

# Target system

A data buffer is allocated on the specified system. If the [`M_DEFAULT_HOST`](../../Reference/buf/MbufAllocColor.md) system is specified, the default Host system of the current Aurora Imaging Library application will be used.

In addition, any operation involving one or more buffers will be performed by the most appropriate system that is associated with one of the buffers. By default, if none of these systems is more appropriate than the Host, the Host is used to perform the operation.
