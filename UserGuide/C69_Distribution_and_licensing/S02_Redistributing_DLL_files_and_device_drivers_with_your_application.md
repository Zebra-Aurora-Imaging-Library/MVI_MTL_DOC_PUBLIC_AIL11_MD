---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distribution_and_licensing
section: Redistributing_DLL_files_and_device_drivers_with_your_application
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / licensing / Redistributing DLL files and device drivers with your application"
---

# Redistributing Aurora Imaging Library or Aurora Imaging Library Lite DLL files and device drivers with your application

To distribute your Aurora Imaging Library or Aurora Imaging Library Lite application, you will have to redistribute the Aurora Imaging Library or Aurora Imaging Library Lite DLL files and the necessary device drivers with your application.

When installing your application on a computer with the same required version of Aurora Imaging Library or Aurora Imaging Library Lite DLL files and the necessary device drivers already installed, your application's setup program must not reinstall Aurora Imaging Library or Aurora Imaging Library Lite. Your application should use the version of Aurora Imaging Library or Aurora Imaging Library Lite already installed on the computer.

Conversely, if the version of the Aurora Imaging Library or Aurora Imaging Library Lite files on the computer is different from your application's required version, you must reinstall the Aurora Imaging Library or Aurora Imaging Library Lite DLL files and device drivers with the version your application requires.

Note that your Aurora Imaging Library development computer should not have Aurora Design Assistant installed if you intend on redistributing an Aurora Imaging Library-only application; this will cause your redistributed Aurora Imaging Library application to have a different update version than your Aurora Imaging Library development computer.

## Redistributing directly from the Aurora Imaging Library or Aurora Imaging Library Lite installation media

If the target computer (on which you want to install the Aurora Imaging Library or Aurora Imaging Library Lite DLLs and device drivers) is immediately accessible, you can install the DLLs directly from the Aurora Imaging Library or Aurora Imaging Library Lite installation media. To do so, run the Aurora Imaging Library or Aurora Imaging Library Lite setup program and uncheck the "Aurora Imaging Library Development" option.

## Redistributing using your own setup program

To redistribute the Aurora Imaging Library or Aurora Imaging Library Lite DLLs and device drivers when using your own setup program, you can have your application's setup program call Aurora Imaging Library or Aurora Imaging Library Lite's redistribution setup program in one of two ways:

- **Interactive redistribution**. Prompts your customer for setup information.
- **Silent redistribution**. Uses a custom response file, instead of prompting your customer for information.

## Determining the required license for your application

The successful distribution of an Aurora Imaging Library application requires the appropriate runtime license for the various modules used within the application. Under Windows, you can use the Aurora Imaging Profiler utility to determine the Aurora Imaging Library license that your application requires for the Aurora Imaging Library package(s) used. The following steps outline how to establish the required Aurora Imaging Library license:

1. Make sure that you have an active Aurora Imaging Library development license, or an Aurora Imaging Library provisional license (that is, evaluation or temporary license).
2. Start an interactive trace using Aurora Imaging Profiler. To create a new trace log in Aurora Imaging Profiler, choose **Generate New Trace** from the **File** menu. For more information concerning generating trace logs in Aurora Imaging Profiler, see [](../C65_Development_and_Debugging_Tools_and_Techniques/S01_Profiler.md).
3. Run your application, making sure to execute all aspects of your application that use Aurora Imaging Library. This needs to include supported Aurora Imaging Library system selections (for example, processing using a GigE Vision, and/or USB3 Vision-based image acquisition), Aurora Imaging Library compression and decompression operations, and Distributed Aurora Imaging Library calls.
4. Terminate the application properly (that is, the [`MappFree`](../../Reference/app/MappFree.md) or [`MappFreeDefault`](../../Reference/app/MappFreeDefault.md) function must execute without errors).
5. In Aurora Imaging Profiler, choose **Show Trace Information** from the **View** menu.

In the above-mentioned window (**Show Trace Information**), you should see the output of the trace-log, along with a list of the Aurora Imaging Library package(s) that were used during the execution of the application. These are the Aurora Imaging Library package(s) that your Aurora Imaging Library license must grant access to successfully run your application in its entirety.

The previous steps are only applicable for applications traced under Windows. To determine the required Aurora Imaging Library package(s) that must be licensed for applications running under Linux, you can either generate a trace-log to file and examine it in Aurora Imaging Profiler (in a Windows environment) using the above-mentioned steps, or use the following command in gencode before starting your application.

```
gencode /m 
```

Once your application has finished its execution, you must stop gencode's monitoring process by pressing the **Enter** key. gencode will then list the Aurora Imaging Library package(s) used by your application. These are the Aurora Imaging Library package(s) that your Aurora Imaging Library license must grant access to successfully run your application in its entirety.
