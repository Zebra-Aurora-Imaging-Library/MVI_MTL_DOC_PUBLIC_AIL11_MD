---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_under_Linux
section: Additional_Linux_notes_Examples_licensing_GUIs_and_display
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Linux / Additional Linux notes Examples licensing GUIs and display"
---

# Miscellaneous user guide information for Linux: Examples, licensing, GUIs, and display

This section includes additional user guide information for Linux regarding examples, licensing, GUIs and display. The information found in this section might be a reiteration of content previously documented.

## Recompiling examples

If you want to recompile the C++ examples in the installation directory, open a console window and type:

```
$ cd $AILDIR11/examples 
$ make clean; make
```

The first example that you should try running is:

```
$ cd MappStart/C++
$./MappStart
```

## Intellicam utility and processing GUIs

Intellicam utility and the processing GUIs are not supported under Linux.

## Display

Buffers with the attribute [`M_DIB`](../../Reference/buf/MbufAlloc2d.md) are not supported.

When using an exclusive display, if you obtain the message "vcf exceeds monitor capabilities" and you are using an old version of the Xorg server, make sure the "Virtual" option in the Display subsection of your xorg.conf file is set appropriately. This option sets the maximum size of the virtual desktop, and should be set to a size that is at least as large as the exclusive display size, established by the selected VCF. In the following example, the virtual desktop size is 2048x2048:

```
SubSection "Display"
         Depth        24
         Modes       "1600x1200" "1280x1024" "1024x768" "800x600" "640x480"
         Modes       "1600x1200" "1280x1024" "1024x768" "800x600" "640x480"
         Virtual       2048 2048
...
```

The maximum size of the virtual desktop depends on the amount of display memory.
