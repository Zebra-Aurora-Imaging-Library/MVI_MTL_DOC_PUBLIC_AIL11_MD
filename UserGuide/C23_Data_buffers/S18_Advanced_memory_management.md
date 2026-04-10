---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Advanced_memory_management
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Advanced memory management"
---

# Advanced memory management

When installing Aurora Imaging Library, the default setup asks the operating system to reserve a portion of RAM from the non-paged memory pool for Aurora Imaging Library. By default, this reserved portion of non-paged memory is a single contiguous block that Aurora Imaging Library typically uses for grab buffers. The default is usually sufficient and no further action is required on your part. On occasion, it might be necessary to alter how Aurora Imaging Library manages this memory. This can occur when you need large buffers and/or a large number of buffers for your application, as well as when you are changing the amount of RAM or the number of PCI devices, such as boards, in your computer.

To make adjustments to Aurora Imaging Library's non-paged memory setup, especially the non-paged memory size reserved for Aurora Imaging Library, use the Aurora Imaging Configurator utility under the **Non-paged memory** item.

## Aurora Imaging Library non-paged memory management driver

The Aurora Imaging Library non-paged memory management driver uses the operating system's mechanism to reserve the total amount of non-paged memory required for Aurora Imaging Library. The mechanism allocates non-paged memory into non-adjacent chunks of contiguous memory. The chunk size depends upon the total RAM installed in your computer, but the size is only relevant if the total non-paged memory requested for Aurora Imaging Library is greater than the size of a single chunk. If you request 1 Gbyte of non-paged memory and, based on the amount of RAM in your computer, the chunk size is 256 Mbytes, you will have 4 non-adjacent 256-Mbyte chunks of contiguous memory. In this example, if you allocate two 100 Mbyte grab buffers, they could both exist in a single chunk, but if you allocate two 150 Mbyte grab buffers, they would each occupy a separate chunk, since a single buffer cannot occupy two separate chunks. Aurora Imaging Library will not allow you to allocate a buffer larger than the chunk size, by default. To allow very large buffers, you must enable large non-paged Aurora Imaging Library buffers using the Aurora Imaging Configurator utility.

> **Note:** Note that you might have installed Aurora Imaging Library without installing the Aurora Imaging Library non-paged memory management driver; this can happen if no boards were selected during Aurora Imaging Library installation. If this is the case, there will be no Aurora Imaging Configurator utility option to adjust non-paged memory, since there is no driver able to reserve non-paged memory for Aurora Imaging Library; consequently, there will be no non-paged memory available for your application. To install the Aurora Imaging Library non-paged memory manager driver, you must uninstall and re-install Aurora Imaging Library.

### Large non-paged Aurora Imaging Library buffers

If your application needs individual buffers that are larger than the chunk size, such as for line-scan cameras, you can explicitly allow the Aurora Imaging Library non-paged memory management driver to re-allocate memory such that chunk sizes are ignored when allocating buffers. However, if afterwards, you change either the PCI devices in your computer, or the total amount of RAM, the original re-allocation of memory that allowed large non-paged Aurora Imaging Library buffers can lead to the problem of memory conflicts and/or an unusable block of memory.

To resolve these problems, you should use a tool in the Aurora Imaging Configurator utility that respects both the amount of non-paged memory you reserved for Aurora Imaging Library and the new hardware setup. This two button tool is found in the **Error Detection** sub-item under the **Non-paged memory** item. Any user can click on the **Check Errors** button to assess if a hardware change created conflicts or waste, but only an administrator can click on the **Repair Errors** button to repair the issue. This tool can also be setup to run automatically immediately after logging in. To do so, select the **Check errors at each reboot and each time the Aurora Imaging Configurator utility starts** option and/or the **Fix problems when found** option. If an error is found while the tool is running, an error message will appear prompting you for further action to repair the issue. If the **Fix problems when found** option was selected and you logged in as an administrator, the problem will be fixed and a message box will appear prompting a reboot.

You must allow the tool to reboot twice for the fix to take effect.

> **Note:** Note that if a conflict occurs that crashes your computer and prevents a normal boot, press and hold **F8** while your computer is booting to enter safe mode. Once in safe mode, open the Aurora Imaging Configurator utility and click on the **Check Errors** button followed by the **Repair Errors** button. When Windows is running in safe mode, the Aurora Imaging Configurator utility's On-Demand mode is enabled by default, and there is no non-paged memory reserved.

### On-Demand Mode

If your Aurora Imaging Library application sporadically needs more non-paged memory than what was reserved, or the maximum usage is unknown and you want to minimize non-paged reservation, increasing the amount of non-paged memory reserved for Aurora Imaging Library might be counterproductive. To accommodate this type of application, enable On-demand mode using the Aurora Imaging Configurator utility; the option can be found through the **Non-paged memory** item. This mode allows Aurora Imaging Library to use RAM outside of the non-paged memory reserved for Aurora Imaging Library. This mode should be used with caution because the memory outside of the non-paged memory reserved for Aurora Imaging Library is taken from the operating system's non-paged memory pool. If the pool is ever exhausted, this might lead to a system crash because of a failed memory allocation by another application or driver.
