---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_multiprocessing_and_under_multithread_systems
section: Multiprocessing
module_tag: thr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / multiprocessing / Multiprocessing"
---

# Multi-processing

Multi-processing is the ability to execute various processes (applications) simultaneously.

Aurora Imaging Library applications are autonomous processes (or executables) designed to execute a complete operation or series of operations. Therefore, they can profit from multi-processing by executing independently, without interference from each other.

All currently supported Aurora Imaging Library systems support multi-processing. This means that the same board can be allocated by more than one process simultaneously; CPU and memory resources will be split between each process as needed. However, many resources (such as individual digitizers on some boards) cannot be allocated by more than one process simultaneously. Refer to the documentation for your particular hardware to determine which resources cannot be allocated by more than one process simultaneously.
