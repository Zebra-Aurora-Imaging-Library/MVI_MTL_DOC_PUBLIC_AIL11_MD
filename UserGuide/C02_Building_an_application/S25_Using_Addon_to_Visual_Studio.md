---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Using_Addon_to_Visual_Studio
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Using Addon to Visual Studio"
---

# Using Aurora Imaging Library's Visual Studio add-on

Aurora Imaging Library provides an extension to Microsoft Visual Studio users to assist in developing Aurora Imaging Library applications. This includes a menu and toolbar added to Microsoft Visual Studio, as well as F1 contextual help and statement completion (IntelliSense) for projects written in C++ or C#. Aurora Imaging Library's Visual Studio add-on is automatically installed whenever the Library is installed on a computer with a supported version of Microsoft Visual Studio. To verify that the extension has been properly installed, open the **Extensions and updates** dialog box in Microsoft Visual Studio; **Aurora Imaging Library's Visual Studio add-on** should appear in the list presented in the dialog box.

By default, all features of the add-on are enabled. In the **Aurora Imaging Library's Visual Studio add-on** pane of the Aurora Imaging Configurator utility (located by expanding the **Benchmarks and Utilities** item and selecting the add-on sub-item in the presented tree structure), you can enable or disable the features of the add-on. This pane provides a checkbox that indicates whether to enable the add-on. In addition, if the add-on is enabled, you can disable statement completion in any of the three supported languages. Disabling any of the checkboxes and clicking on the **Apply** button will disable those features. In this way, you can also install the add-on if it was not previously installed, which can be used if you install Microsoft Visual Studio after installing the library and want to use the extension. Microsoft Visual Studio must be closed whenever changing the extension's settings.

## Aurora Imaging Library menu and toolbar

The Aurora Imaging Library's Visual Studio add-on adds a menu to Microsoft Visual Studio's menu bar. This menu allows you to access the library tools, adjust your preferences, read documentation about the add-on, and change the settings of the add-on in the Aurora Imaging Configurator utility.

Additionally, you can add a toolbar to your Microsoft Visual Studio window that contains links to the library tools. The toolbar is not visible by default. See the Microsoft Visual Studio help to add the toolbar.

*[Image: add-onToolbar.png]*

## F1 contextual help

The Aurora Imaging Library's Visual Studio add-on provides F1 contextual help for the library's code. To see the library's Reference page for a particular function, place your cursor on or highlight the function and press F1. This will open the library's Help file in a window that is part of the current Microsoft Visual Studio instance. Note that this Help file will have the default saved settings (such as, Aurora Imaging Library systems or Product settings) as that of the installed library's Help and cannot save different (custom) settings.

To see the information for a specific Aurora Imaging Library value, place your cursor on or highlight the value and press F1. The value must be used in the correct contextual location of its corresponding function and parameter. For example, if you want to see the description for the [`M_BACKGROUND_COLOR`](../../Reference/disp/MdispControl.md) value of [`MdispControl`](../../Reference/disp/MdispControl.md), [`M_BACKGROUND_COLOR`](../../Reference/disp/MdispControl.md) must be passed to the second parameter of [`MdispControl`](../../Reference/disp/MdispControl.md). Contextual help only works on the library's values (all values that begin with [`M...`](../../Reference/3dmap/M3dmapAlloc.md)); it will not work on text strings (values encapsulated in _M_TEXT_) and user-defined variables or numerical values. If contextual help is requested for a value and the value cannot be found, the Reference page for the function will be opened instead.

Contextual help can also be requested for Aurora Imaging Library custom data types (for example, _M_ID_) or a portability function (for example, `MosMain()`). If you place your cursor or highlight a custom data type or portability function, the Help file opens [Aurora Imaging Library custom data types, extensions, and portability functions](S16_Custom_data_types_extensions_and_portability_functions.md).

> **Note:** Note that when you first open the library's Help with F1, you might need to wait a few moments before the Help file opens. Also if you are opening the Help for the first time, it might open [Welcome](../C00_AIL_Introduction/S01_Welcome.md).

## Statement completion

The Aurora Imaging Library's Visual Studio add-on provides statement completion to users writing the library's code in C++ and C#. Statement completion makes developing with the library easier by providing quick access to the library functions and values through IntelliSense. Statement completion will work on any file that Microsoft Visual Studio recognizes as a C++ or C# file.

When you start typing in a file for the supported languages in Microsoft Visual Studio, the IntelliSense session will display all available library functions. Alternatively, you can use Ctrl + Spacebar to start an IntelliSense session. To add a function from the displayed list, highlight it and then press the Tab key.

If a function's parameter can take a library value as possible settings, IntelliSense can display a list of suggested values that you can use. When an IntelliSense session is opened upon specifying a parameter setting in a function, a tab will appear in the IntelliSense list called **Suggested Aurora Imaging Library values**. If the **Prioritize Suggested Aurora Imaging Library Values** item under **Preferences** in the **AIL** menu is enabled, this tab will appear by default. If it is not the default tab shown, click on the **Suggested Aurora Imaging Library Values**tab or press Alt +**.** to bring up all the documented values that can be used as a parameter setting. If you highlight an item in the displayed list, a tooltip will display the description for that value, the installed library systems it supports, and any possible combination values (if available). If [`M_DEFAULT`](../../Reference/app/MappAlloc.md) is available, it will be listed first. To add a value from the displayed list, highlight it and press the Tab key. The descriptions in the Help file might be more up-to-date than those presented in the IntelliSense tooltip.

*[Image: StatementCompletionCPP.png]*

Note that **Suggested Aurora Imaging Library values** are not fully supported through IntelliSense when importing static members in C# with the using static directive (`using static Zebra.AuroraImagingLibrary.AIL;`). For parameters that require an Aurora Imaging Library constant in C#, IntelliSense will only suggest values when using explicit qualifications at the parameter level. The following example demonstrates this.

```
AIL.M3ddispSetView(SceneDisplay, AIL.M_AUTO, AIL.M_TOP_TILTED, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);
```

When explicitly typing the`AIL.` prefix at the parameter level, IntelliSense will suggest a constant for the specific parameter (for example, `AIL.M_AUTO`). For parameters that require a string, IntelliSense will suggest values appropriately.

If you start an IntelliSense session and a suggested value has a combination value available, IntelliSense will say `(combinations available)` next to the value's name and the tooltip will list the possible combination values.

*[Image: CombinationsListCPP.png]*

If you select a value with combinations available and you type a `+`, you can start a new IntelliSense session that presents a list of the available combinations. Note, however, that if a combination value has combinations available, they will not be listed in a new IntelliSense session.

*[Image: CombinationSuggestionCPP.png]*

If the value of a parameter is dependent on the value of another parameter, you must specify the parameter on which it is dependent first. For example, possible values in [`MdigControl`](../../Reference/dig/MdigControl.md) for the [`ControlValue`](../../Reference/app/MappControl.md) parameter are dependent on the setting of the [`ControlType`](../../Reference/app/MappControl.md) parameter, so you must specify a value for the [`ControlType`](../../Reference/app/MappControl.md) parameter for IntelliSense to present a list of the possible values for the [`ControlValue`](../../Reference/app/MappControl.md) parameter.

*[Image: AssociationSuggestionCPP.png]*

IntelliSense will only suggest values for input parameters and for values listed in a table. Values that have not been formally documented (for example, a value mentioned in the release note of a driver update) will not be displayed in IntelliSense. Any value that is not supported on the systems that you have installed will be filtered by default; to change this behavior, disable the **Filter Suggested Aurora Imaging Library Values by Installed Systems**option under **Preferences** in the **AIL** menu.

> **Note:** Although values are suggested, values such as an integer value might be possible as a parameter setting, but are not listed in IntelliSense. See the appropriate reference page for more information.
