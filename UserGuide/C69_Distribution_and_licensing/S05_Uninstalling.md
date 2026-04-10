---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distribution_and_licensing
section: Uninstalling
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / licensing / Uninstalling"
---

# Uninstalling

To uninstall Aurora Imaging Library or Aurora Imaging Library Lite, and all other Aurora Imaging products, call the Aurora Imaging products uninstall program.

If you are working in Windows, see [Enabling 8.3 file name creation](../C02_Building_an_application/S02_Installation.md) for additional information.

To uninstall in silent mode, perform the following:

1. Find the silent uninstall program, _silentuninstall&lt;version number>.exe_, located in the Files directory of the installation media (for example, _&lt;DVD Root>\Files\silentuninstall&lt;version number>.exe_).
2. Copy the _silentuninstall&lt;version number>.exe_ file from the installation media to a different folder on the computer, ensuring that this new location is not the folder where Aurora Imaging Library is installed (the uninstall program must not be present in the installation directory).
3. Change the working directory to the location where you have copied the _silentuninstall&lt;version number>.exe_ file.
4. Call the silent uninstall executable file:
   ```
   silentuninstall<version number>.exe
   ```
