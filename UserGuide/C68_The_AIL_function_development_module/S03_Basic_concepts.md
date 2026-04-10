---
doctype: UserGuide
part: "Miscellaneous"
chapter: The_AIL_function_development_module
section: Basic_concepts
module_tag: func
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / function / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Function Development module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Function Development module are:

- **Asynchronous function**. A function that returns control to the calling thread before it has finished executing.
- **C-based user-defined Aurora Imaging Library function**. A user-defined function written in C or C++, compiled into a DLL, and integrated into an Aurora Imaging Library application using the Aurora Imaging Library function development module.
- **Callee function**. In a user-defined Aurora Imaging Library function, the callee function is called by the caller function. The callee function performs the data processing operations of the user-defined Aurora Imaging Library function.
- **Callee processor**. The callee processor is the processor on which the callee function will be executed. The callee processor can be either the same processor as the caller processor or a remote processor.
- **Caller function**. In a user-defined Aurora Imaging Library function, the caller function provides the user interface. The callee function is called from within the caller function.
- **Caller processor**. The caller processor is the processor on which the caller function is executed.
- **Distributed processing**. Distributed processing is a processing method which involves using more than one processor to perform the required operations. Execution of a function by a remote processor requires special consideration during compilation.
- **Opcode**. All Aurora Imaging Library functions, including user-defined functions, are associated with a unique operation code. Operation codes are, in turn, associated with pointers to their corresponding functions. The operation code is then used to refer to the function indirectly. For simplicity, operation codes are referred to as opcodes.
- **Opcode table**. The opcode table is a table in which on-board processors store the associations between opcodes and pointers to Aurora Imaging Library functions.
- **Remote processor**. A remote processor is any available processor which is separate from the Host processor.
- **Script-based user-defined Aurora Imaging Library function**. A user-defined function written in a language requiring an interpreter or based on the .NET framework, and integrated into an Aurora Imaging Library application using the Aurora Imaging Library function development module.
- **Synchronous function**. A function that returns control to the calling thread only after it has finished executing.
