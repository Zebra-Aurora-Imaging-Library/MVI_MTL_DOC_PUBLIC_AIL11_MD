---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_NET
section: Memory_management_while_using_AIL_with_NET
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / NET / Memory management while using AIL with NET"
---

# Memory management while using Aurora Imaging Library with

One of the key features of writing code in a managed environment is the garbage collector. To move from writing unmanaged C/C++ to managed C#, it is important to understand how memory is managed in C#.

The .NET garbage collector is responsible for the allocation and release of objects in managed memory. Whenever an object is initialized using the `New` operator, the garbage collector allocates memory for that object and tracks its usage within your application. To free memory, the garbage collector sweeps through managed memory and frees the memory allocated to all objects that are no longer in use by your application. The timing of a garbage collection is unpredictable; it is possible, however, to force a garbage collection using the .NET System.GC.Collect function.

## M...Free() versus finalizers

Objects that rely on a garbage collection scheme in the .NET environment have nondeterministic finalizers. These finalizers are called when the garbage collector decides to delete the object and not when the object is out of scope. Therefore, you cannot rely on having your resources freed by the object's finalizer since you do not know when the finalizers will be called.

You should trap errors that might cause your call to an [`M...Free`](../../Reference/buf/MbufFree.md)function to be skipped. Any code between the allocation of an Aurora Imaging Library object and the[`M...Free`](../../Reference/buf/MbufFree.md)function calls should be placed in a Try block, while an [`M...Free`](../../Reference/buf/MbufFree.md) function should be placed in a Finally block.

## Invoking the garbage collector

To manually invoke the garbage collector, use the .NET System.GC.Collect function. The garbage collector disposes of all objects that are out of scope and that are no longer referenced by any active pointer. For more information on the system.GC class, refer to the .NET documentation available through the MSDN library.

> **Note:** Note that it is good practice to let the garbage collector choose the most appropriate time to run.

## Pinning memory to be used by MbufCreate...()

[`MbufCreate2d`](../../Reference/buf/MbufCreate2d.md) and [`MbufCreateColor`](../../Reference/buf/MbufCreateColor.md) create an Aurora Imaging Library buffer that contains a pointer to a previously allocated block of memory. In a .NET application, memory is managed by the common language runtime's garbage collector, so steps must be taken to protect the block of memory from being moved or deleted, otherwise the buffer might point to random (garbage) data. To protect the block of memory, use the `GCHandle` structure to create a pinned object that contains the block of memory; a pinned object is not moved or deleted by the garbage collector, and so protects the block of memory it contains. The following code demonstrates how to create a pinned object to encapsulate a previously allocated block of memory, and how to create a buffer, using [`MbufCreate...`](../../Reference/buf/MbufCreate2d.md), that has a pointer to this protected block of memory.

> **Code example:** [userguide.mil_in_net.memory_management.cs](userguide.ail_in_net.memory_management.cs)
