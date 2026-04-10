---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_multiprocessing_and_under_multithread_systems
section: Transparent_MultiCore_Use
module_tag: thr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / multiprocessing / Transparent MultiCore Use"
---

# Transparent multi-core use

When dealing with a system allocated on a computer with multiple CPU cores (processors), Aurora Imaging Library can transparently split the processing work of an Aurora Imaging Library function between the CPU cores available to the process running your Aurora Imaging Library application. Typically, this division of labor can greatly increase the overall execution speed of Aurora Imaging Library functions.

When multi-core processing is enabled for a thread (and supported in hardware), the work of most Aurora Imaging Library functions on the thread is split into several parts and transparently sent to multiple CPU cores. When the work is done on all parts, the function returns and the result is made available.

The goal of multi-core processing is to increase the function's processing speed. The following is a list of characteristics that can affect the speed gained using multi-core processing:

- **Original speed**. The splitting and merging of the work adds a certain amount of overhead to any function performed using multi-core processing. Therefore, if the original function is normally very fast, performing the function using multi-core processing might not provide a speed increase. This is often observed when processing small buffers. Aurora Imaging Library tries to dynamically determine the best multi-core processing fit for the current function call to minimize the overhead; but it cannot be eliminated.
- **I/O access**. If the function has a high ratio of I/O accesses (such as, memory accesses) compared to the amount of actual processing, the function is I/O dependent. In such a case, performing the function using multi-core processing can cause all the CPU cores involved to fight for access to the same resource (such as, memory). The result is a smaller gain than expected and sometimes even less performance than the original single-core execution.
- **Computer architecture**. There are a lot more variables affecting the performance of a multiple CPU core host computer than a traditional single processor host computer. Each of these variables (for example, CPU, cache, bus, and memory) becomes more important in the final performance for each additional processor. Therefore, a small change in the architecture of the host computer can easily result in very different performance numbers.
- **Scalability**. Usually, multi-core processing will get faster as the number of CPU cores increases, but will gain less and less as more CPU cores are added. Often, the processing performance will even decrease beyond a certain number of CPU cores. This behavior is as much dependent on the hardware architecture as on the software, and nothing can be done to have linear scalability throughout the whole range of hardware and software requirements. Aurora Imaging Library tries to reduce the performance decreases by applying predictive throughput algorithms, but cannot eliminate all possible performance losses as the number of CPU cores gets large.

You can enable and change the multi-core processing default settings using the Aurora Imaging Configurator utility and programmatically using [`MappControlMp`](../../Reference/app/MappControlMp.md) and [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) functions.

## Setting the maximum number of CPU cores per thread

You can manage the CPU core utilization of the multi-core processing part of Aurora Imaging Library functions on each thread in an Aurora Imaging Library application. By default, when enabled, multi-core processing spreads processing among all the CPU cores available to the process running the Aurora Imaging Library application. In most cases, this provides good performance gains without further configuration. In some cases, for instance when using multiple threads, spreading multi-core processing among all the available CPU cores might lead to multiple threads requesting the same CPU cores at the same time. This might reduce the overall efficiency of your Aurora Imaging Library application.

To help reduce this problem, you can use [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_CORE_MAX`](../../Reference/app/MappControlMp.md) or [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_CORE_MAX`](../../Reference/thr/MthrControlMp.md). The former control type sets the default maximum number of CPU cores to which each thread can spread its multi-core processing, while the latter applies the same setting only to the specified thread. You should only adjust the [`M_CORE_MAX`](../../Reference/app/MappControlMp.md) control types if using multiple threads and fine-tuning the performance of your Aurora Imaging Library application. The total number of CPU cores available to run the multi-core processing part of Aurora Imaging Library functions on a thread is always limited by your operating system and the number of CPU cores installed in your computer.

> **Note:** Note that, the first call to [`MappAlloc`](../../Reference/app/MappAlloc.md) or [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) determines the number of CPU cores available from the operating system. This information is stored in Aurora Imaging Library and not updated dynamically. Changing the number of processors available at the operating-system level, after your application is allocated in Aurora Imaging Library, can result in erratic and unpredictable behavior.

If, for example, the process has access to eight CPU cores and it has two processing threads, with multi-core processing enabled, each thread will try to spread its multi-core processing among all eight CPU cores. However, it might be more efficient to split the CPU cores between the two threads. If the threads have similar processing loads, you could restrict each thread to use four CPU cores, using [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_CORE_MAX`](../../Reference/app/MappControlMp.md) set to four. This configuration is typically more efficient than allowing both threads access to all eight CPU cores.

If one thread is processing-intensive while the other is not, you could give the processing-intensive thread access to the majority of the available CPU cores using [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_CORE_MAX`](../../Reference/thr/MthrControlMp.md). For example, if the process running your application has access to 8 CPU cores, you could give your processing-intensive thread access to seven CPU cores, and the other thread access to a single CPU core. Note that Aurora Imaging Library might not be the only application running that requires some form of processing; in this case, you should limit the number of CPU cores your Aurora Imaging Library application uses, using [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_CORE_MAX`](../../Reference/app/MappControlMp.md). This has the effect of limiting the number of cores each thread in your application can use.

## Controlling where and how the processing occurs

In special circumstances, and for extreme optimization on very stable computing platforms, other options are available to control multi-core processing. Note that in many cases, the operating system might interfere with these controls. Therefore, it is often safer, and sometimes even faster, to leave them at their default settings. Furthermore, these controls are very dependent on the underlying hardware architecture, and their successful use can only be accomplished with a high degree of knowledge of the architecture details.

### Core affinity

You can control exactly which CPU cores are used for multi-core processing within the entire application or for a specific thread using core affinities. A core affinity indicates on which CPU core(s) (processor(s)) should the scheduler of the operating system allow processing. In Aurora Imaging Library, core affinity is represented by a bit-mask, where each bit represents the index of a CPU core. When a bit is set (1) at a specific position, multi-core processing is allowed to occur on that CPU core. When a bit is not set (0), multi-core processing is not allowed to occur on that CPU core. CPU cores always have the same indices, as long as the hardware in your computer and the operating system does not change.

In Aurora Imaging Library, core affinity bit-masks are passed in an array of _AIL_UINT64_ integers, where the least-significant bit of the first element represents CPU core 0, and the most-significant bit, CPU core 63. The least-significant bit of the second element (if needed), represents CPU core 64, and the most-significant bit, CPU core 127. This follows similarly for all of the following elements in the array. A normal core affinity bit-mask should have at least one bit enabled so that at least one CPU core is enabled for processing. A core affinity bit-mask whose bits are all set to zero is therefore a special case and represents the default setting of all CPU cores being enabled for processing.

You can set the core affinity bit-mask using [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_CORE_AFFINITY_MASK`](../../Reference/app/MappControlMp.md) or [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_CORE_AFFINITY_MASK`](../../Reference/thr/MthrControlMp.md), where the latter only applies to the multi-core processing part of Aurora Imaging Library functions on the specified thread.

In addition, if an Intel processor is used with hyper-threading enabled, you can execute as if hyper-threading was disabled and restrict multi-core processing to one logical core per physical CPU core (hyper-threading), minimizing logical core interactions. To do so, use [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_CORE_SHARING`](../../Reference/app/MappControlMp.md) or [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_CORE_SHARING`](../../Reference/thr/MthrControlMp.md), where the latter only applies to the multi-core processing part of Aurora Imaging Library functions on the specified thread. Note that the process or thread might not have exclusive access to the CPU cores; other processes or threads might still use the other logical cores of a physical CPU core and might impede multi-core processing restricted this way.

In instances where you have multiple independent processing threads (for instance, when you have a multi-camera setup), it is advised to limit the interaction of the threads in each core. This can be done by setting the core affinity mask for each thread ([`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_CORE_AFFINITY_MASK`](../../Reference/thr/MthrControlMp.md)) and explicitly limiting the CPU cores used by each processing thread, taking care to avoid core-use overlap.

Note that [`M_CORE_MAX`](../../Reference/app/MappControlMp.md), [`M_CORE_AFFINITY_MASK`](../../Reference/app/MappControlMp.md), and [`M_CORE_SHARING`](../../Reference/app/MappControlMp.md) reduce the number of effective CPU cores to only those that meet the requirements of all three specifications.

### Priority

You can set the processing priority of the multi-core processing part of Aurora Imaging Library functions. To do so, use [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_MP_PRIORITY`](../../Reference/app/MappControlMp.md) or [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_MP_PRIORITY`](../../Reference/thr/MthrControlMp.md). In the latter case, only the multi-core processing part of Aurora Imaging Library functions on the specified thread will be affected. Any priority set this way only affects multi-core processing. To set the priority of a thread, use [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_THREAD_PRIORITY`](../../Reference/thr/MthrControl.md).

### NUMA Support

Non-uniform memory access (NUMA) is a computer architecture introduced by AMD, but has now been adopted by Intel as well. It is the use of multiple memory banks in a computer system, where each memory bank is local to a subset of the available processing CPU cores. Memory accesses are more efficient when using memory banks that are local to their respective CPU cores.

NUMA is supported in Aurora Imaging Library; you can allocate buffers in specific memory banks. Note that the explicit use of memory banks is an advanced optimization and scalability technique, and you must have a full understanding of the underlying computer architecture to successfully use it.

The typical way to use NUMA is to first inquire information to help decide which memory bank to use.

You can inquire about the memory banks that are local to the CPU cores available to the process running your application, using [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_MEMORY_BANK_NUM`](../../Reference/app/MappInquireMp.md) and [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_MEMORY_BANK_AFFINITY_MASK`](../../Reference/app/MappInquireMp.md). The former returns the number of memory banks local to the process's CPU cores, while the latter returns a memory bank affinity mask representing the memory banks local to the process's CPU cores.

You can inquire about the memory bank local to a specified CPU core using [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_CORE_MEMORY_BANK`](../../Reference/app/MappInquireMp.md). This returns the index of the memory bank local to the specified core. As well, you can inquire about the CPU cores local to a specified memory bank, using [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_MEMORY_BANK_CORE_AFFINITY_MASK`](../../Reference/app/MappInquireMp.md). This inquire type returns a core affinity mask representing the CPU cores local to the specified memory bank.

When enough memory bank information is known, you can allocate buffers in more efficient memory banks using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_MEMORY_BANK_n`](../../Reference/buf/MbufAlloc1d.md), where _n_ is the index of the memory bank.

## Basic steps to using multi-core processing

The following steps provide a basic methodology for using multi-core processing controls and inquires:

1. Enable multi-core processing using either the Aurora Imaging Configurator utility or [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_MP_USE`](../../Reference/app/MappControlMp.md) set to [`M_ENABLE`](../../Reference/app/MappControlMp.md).
2. Optionally, set the maximum number of CPU cores to use per thread, using either the Aurora Imaging Configurator utility or [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_CORE_MAX`](../../Reference/app/MappControlMp.md).
   Note that the effective number of CPU cores is always limited by the number of CPU cores available to the process running your Aurora Imaging Library application, as per your operating system. To determine the number of CPU cores available to the process, use [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_CORE_NUM_PROCESS`](../../Reference/app/MappInquireMp.md).
3. Optionally, disable multi-core processing for one or more threads using [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_MP_USE`](../../Reference/thr/MthrControlMp.md) set to [`M_DISABLE`](../../Reference/thr/MthrControlMp.md).
4. Optionally, set the maximum number of CPU cores to use for a specific thread using [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_CORE_MAX`](../../Reference/thr/MthrControlMp.md).
   To determine the effective number of CPU cores that a specific thread can use, use [`MthrInquireMp`](../../Reference/thr/MthrInquireMp.md) with [`M_CORE_NUM_EFFECTIVE`](../../Reference/thr/MthrInquireMp.md).

> **Note:** Note that, alternatively you can disable multi-core processing at the application level (using [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_MP_USE`](../../Reference/app/MappControlMp.md) set to [`M_DISABLE`](../../Reference/app/MappControlMp.md)) and enable it for a specific thread (using [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_MP_USE`](../../Reference/thr/MthrControlMp.md) set to [`M_ENABLE`](../../Reference/thr/MthrControlMp.md)).
