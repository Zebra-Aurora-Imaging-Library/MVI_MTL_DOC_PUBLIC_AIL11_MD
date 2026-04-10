---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distribution_and_licensing
section: Licenses
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / licensing / Licenses"
---

# Aurora Imaging Library and Aurora Imaging Library Lite licenses

Aurora Imaging Library and Aurora Imaging Library Lite have different licensing models:

- **Aurora Imaging Library Lite.** With Aurora Imaging Library Lite, there is a permanent development license (for Aurora Imaging Library Lite functionality) provided with Zebra hardware or when an Aurora Imaging Library Lite supplemental license is present.
- **Aurora Imaging Library.** With Aurora Imaging Library, there are two types of permanent licenses: a development license and/or a runtime license. Licenses are always verified when an Aurora Imaging Library application is allocated, as well as when the application is running (this verification is designed to have no measurable impact on performance). You can use an Aurora Imaging Library provisional license while waiting to obtain a permanent license. Users with Aurora Imaging Library installed also have the ability to run (but not debug) Aurora Imaging Library Lite applications if Zebra boards are present.

Refer to the respective Zebra software license agreement for the legal provisions of using/redistributing Aurora Imaging Library or Aurora Imaging Library Lite. The rest of this chapter deals with Aurora Imaging Library and Aurora Imaging Library Lite and its licensing mechanisms.

## Aurora Imaging Library provisional licenses

Aurora Imaging Library provisional licenses permit you to temporarily use Aurora Imaging Library either while waiting for a permanent license, or to evaluate whether to buy Aurora Imaging Library. The following table outlines the types of Aurora Imaging Library provisional licenses and their function and purpose:

| Name of license | Function | Purpose |
| --- | --- | --- |
| Aurora Imaging Library evaluation license | Permits you to run and debug Aurora Imaging Library applications for 30 days. | To try out Aurora Imaging Library's features, and evaluate whether you want to purchase an Aurora Imaging Library permanent license. |
| Aurora Imaging Library temporary license | Permits you to run Aurora Imaging Library applications for 30 days. | To run Aurora Imaging Library applications while waiting for a hardware or software license-key to activate a permanent runtime license. |

### Aurora Imaging Library evaluation license

An Aurora Imaging Library evaluation license is a 30-day development license. The evaluation license permits you to run and debug Aurora Imaging Library applications. You should use an Aurora Imaging Library evaluation license to try out Aurora Imaging Library's features, and evaluate whether you want to purchase a permanent license.

The evaluation license needs to be activated using a code obtained from the Aurora Imaging Library product page on the Zebra website or from a local representative, and will be valid for 30 days from the date of issue. This code must be entered in the Aurora Imaging Configurator utility application, in the **Activate** pane, accessible from the **Licensing** item.

Attempting to alter the computer's calendar before the evaluation license expires will immediately disable Aurora Imaging Library. In that event, Aurora Imaging Library can only be reused once an appropriate license is installed.

Note that an Aurora Imaging Library evaluation license is release-specific. Once activated, it works only with the version for which it was issued.

### Aurora Imaging Library temporary license

An Aurora Imaging Library temporary license is a 30-day runtime license that permits you to run Aurora Imaging Library applications. It does not allow for development (debugging) of Aurora Imaging Library applications. You should use an Aurora Imaging Library temporary license to run Aurora Imaging Library applications while waiting for an Aurora Imaging Library runtime dongle or a software license-key to activate a permanent runtime license.

The temporary license is only available on certain Zebra hardware. The license is activated from the Aurora Imaging Configurator utility application, in the **Temporary License** pane, accessible from the **Licensing** item. The license is valid for 30 days from the moment the license is activated with the Aurora Imaging Configurator utility.

Attempting to alter the computer's calendar before the temporary license expires will immediately disable Aurora Imaging Library. In that event, Aurora Imaging Library can only be reused once an appropriate license is installed.

Note that an Aurora Imaging Library temporary license is release-specific. Once activated, it works only with the version for which it was issued. While it can be reactivated for a future release, it cannot be used with a prior release. Activating a temporary license for a newer release makes it invalid for any earlier versions, including the one for which it was originally issued.

## Aurora Imaging Library permanent licenses

Aurora Imaging Library permanent licenses permit you to use Aurora Imaging Library in perpetuity (that is, for an indefinite period of time). The following table outlines the types of Aurora Imaging Library permanent licenses and their function and purpose:

| Name of License | Function |
| --- | --- |
| Aurora Imaging Library development license | Allows you to develop, debug, and run Aurora Imaging Library applications. |
| Aurora Imaging Library runtime license | Allows you to run Aurora Imaging Library applications. |

### Aurora Imaging Library development license

An Aurora Imaging Library development license permits you to develop, debug, and run Aurora Imaging Library applications. To activate the license, an Aurora Imaging Library development dongle (hardware license-key) must be attached to your computer's USB port. An Aurora Imaging Library development dongle is included with the Aurora Imaging Library development package.

Note that the same development license can be used with Aurora Imaging Library X and 11.

### Aurora Imaging Library runtime license

An Aurora Imaging Library runtime license is required for each computer that will run an Aurora Imaging Library application. It does not allow for development (debugging) of Aurora Imaging Library applications.

You can purchase an Aurora Imaging Library runtime license according to the requirements of your application. An Aurora Imaging Library runtime license grants access to specific modules/features, some of which are grouped together in packages. Consult the Aurora Imaging Library datasheet or your local representative regarding the available runtime packages.

An Aurora Imaging Library runtime license can be activated using one of two methods:

- An Aurora Imaging Library runtime dongle (hardware license-key).
- A software license-key.

When using a software license-key, you can hide the Aurora Imaging Library runtime licensing process from your customer using the gencode command-line utility. For more information, see [Hiding the Aurora Imaging Library licensing process](S09_Hiding_the_licensing_process.md). Note that gencode does not work in Linux.

Note that the same runtime dongle can be used with Aurora Imaging Library X and 11. A software license-key purchased for Aurora Imaging Library X can be used with Aurora Imaging Library 11, but not the other way around. If using a software license-key, the associated hardware component must be supported by the Aurora Imaging Library version and installed during setup for the license to work.

## Summary of activation procedures for all Aurora Imaging Library licenses

The following table outlines the activation procedures required for all Aurora Imaging Library licenses on different operating systems:

| Type of Aurora Imaging Library license | Activation procedure |
| --- | --- |
| Evaluation license | License is activated using the Aurora Imaging Configurator utility, in the **Activate** pane, accessible from the **Licensing** item. | License is activated using the Zebra smart camera portal website. |
| Temporary license | License is activated using the Aurora Imaging Configurator utility, in the **Temporary License** pane, accessible from the**Licensing** item. | License is activated using the Zebra smart camera portal website. |
| Development license | Requires an Aurora Imaging Library development dongle (hardware license-key). | Not applicable. [^CEnote] |
| Runtime license | Requires an Aurora Imaging Library runtime dongle (hardware license-key) or software license-key. | Requires a software license-key. [^CEnote] |

[^Smart_camera]: Windows Embedded Compact is supported only for Zebra smart cameras.

[^CEnote]: Under Windows Embedded Compact, there is only one type of permanent license: a runtime license. When you purchase this license, you receive both development and runtime privileges. To activate this license, you must have a software license-key.

## Aurora Imaging Library Lite licenses

The following licenses will permit you to use Aurora Imaging Library Lite functionality on your computer.

### Aurora Imaging Library Lite development license

Aurora Imaging Library Lite comes with a permanent development license (for Aurora Imaging Library Lite functionality) provided Zebra hardware or an Aurora Imaging Library Lite supplemental license is present.

Note that the same development license can be used with Aurora Imaging Library Lite X and 11.

### Aurora Imaging Library Lite supplemental license

You can purchase Aurora Imaging Library Lite supplemental licenses according to the requirements of your application. An Aurora Imaging Library Lite supplemental license grants access to specific modules/features that are otherwise not accessible in Aurora Imaging Library Lite, some of which are grouped together in packages. An Aurora Imaging Library Lite supplemental license grants access to an Aurora Imaging Library Lite development license, if Zebra hardware is not present. Consult the Aurora Imaging Library datasheet, your local representative, or Zebra sales regarding the available packages.

The following Aurora Imaging Library Lite supplemental licenses are available:

- Distributed Aurora Imaging Library.
- Compression/decompression.
- GigE Vision/USB3 Vision.

An Aurora Imaging Library Lite supplemental license can be activated using one of two methods:

- An Aurora Imaging Library Lite supplemental license dongle (hardware license-key).
- A software license-key.

When using a software license-key, you can hide the Aurora Imaging Library Lite supplemental licensing process from your customer using the gencode command-line utility. For more information, see [Hiding the Aurora Imaging Library licensing process](S09_Hiding_the_licensing_process.md). Note that gencode does not work in Linux.

Note that the same supplemental license dongle can be used with Aurora Imaging Library Lite X and 11. A software license-key purchased for Aurora Imaging Library Lite X can be used with Aurora Imaging Library Lite 11, but not the other way around. If using a software license-key, the associated hardware component must be supported by the Aurora Imaging Library Lite version and installed during setup for the license to work.

## Summary of Aurora Imaging Library and Aurora Imaging Library Lite licenses

The following table outlines the license requirements for different Aurora Imaging Library and Aurora Imaging Library Lite activities:

| Activity | Requirement |
| --- | --- |
| Developing an Aurora Imaging Library application | - Aurora Imaging Library development license.<br/>- Aurora Imaging Library evaluation license. |
| Running an Aurora Imaging Library application | - Aurora Imaging Library runtime license.<br/>- Aurora Imaging Library temporary license. |
| Developing an Aurora Imaging Library Lite application | - Aurora Imaging Library Lite installed with any Zebra imaging board or an Aurora Imaging Library Lite supplemental license. |
| Running an Aurora Imaging Library Lite application | - Aurora Imaging Library Lite installed with any Zebra imaging board or an Aurora Imaging Library Lite supplemental license.<br/>- Aurora Imaging Library installed with any Zebra imaging board. |

> **Note:** There are some considerations to take into account regarding licensing when running applications that use Distributed Aurora Imaging Library. For more information regarding licensing of a Distributed Aurora Imaging Library cluster, see [Licensing issues](../C63_Distributed_AIL/S03_Preparing_computers_for_Distributed_AIL.md).

> **Note:** To determine the modules and/or features for which your application has a valid license, use [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_LICENSE_MODULES`](../../Reference/app/MappInquire.md).
