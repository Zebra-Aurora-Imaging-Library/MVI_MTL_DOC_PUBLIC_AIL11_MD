---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distribution_and_licensing
section: Software_licensekey
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / licensing / Software licensekey"
---

# Software license-key for activating runtime license

A software license-key is used to activate an Aurora Imaging Library runtime license. The software license-key is derived from a lock code that is generated using the hardware fingerprint from one of several hardware components on the target computer. This lock code will then be used to obtain a software license-key and activate your Aurora Imaging Library license.

Note that the license is stored on the hardware component that you have chosen. If you install the hardware component on another computer, the license will be available on that computer.

If you purchased a software license-key for Aurora Imaging Library X, you can use the same software license-key with Aurora Imaging Library 11. The associated hardware component must be supported by the Aurora Imaging Library version and installed during setup for the license to work. Note that software license-keys purchased for Aurora Imaging Library 11 are not backwards-compatible with Aurora Imaging Library X.

> **Note:** If you replace the hardware component with the fingerprint that you have chosen, you will need to obtain a new license.

If your computer does not have any hardware component that can act as a fingerprint, you can purchase an **ID dongle** and use it as your hardware component instead. This gets attached to your computer's USB port, and it will allow you to generate a lock code that you can use to create a software license.

You can activate a license using a software license-key as follows:

1. Open Aurora Imaging Configurator from the Aurora Imaging Control Center. Expand the **Licensing** item in the tree structure of the presented interface and then click on the **Generate** sub-item. In the pane to the right of the tree structure, select the type of Aurora Imaging Library license to generate and any additional packages you want to license.
   > **Note:** Since the Aurora Imaging Library Image Analysis package is included in the Aurora Imaging Library Machine Vision package, these two packages are mutually exclusive.
2. Select the hardware component from which to retrieve the fingerprint, from the **License Fingerprint** drop-down list box. After choosing the hardware component, click on the **Generate** button. This will generate a lock code.
3. After generating a lock code, obtain a software license-key by contacting your local representative or using the Zebra licensing portal website.
4. Activate the license by entering the software license-key in the **Activate** pane of Aurora Imaging Configurator, accessible from the **Licensing** item.
