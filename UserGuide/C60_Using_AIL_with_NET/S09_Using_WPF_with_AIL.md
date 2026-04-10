---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_NET
section: Using_WPF_with_AIL
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / NET / Using WPF with AIL"
---

# Using WPF with Aurora Imaging Library

The AIL.NET wrapper allows you to display Aurora Imaging Library image buffers using Windows Presentation Foundation (WPF). The following steps explain how to add WPF support to an Aurora Imaging Library  .NET project:

1. Add the Zebra Aurora Imaging Library NuGet package in your project.
2. Add the following prefix declaration to the root tag of your XAML file to map an XML namespace to the Aurora Imaging Library WPF Display control's assembly:
   ```
   xmlns:ailwpf="clr-namespace:Zebra.AuroraImagingLibrary.WPF;assembly=Zebra.AuroraImagingLibrary.WPF"
   ```
3. Add an &lt;ailwpf:AILWPFDisplay> tag to your XAML page.
4. Associate the AILWPFDisplay control with an Aurora Imaging Library [`M_WPF`](../../Reference/disp/MdispAlloc.md) display, using the DisplayId property. WPF provides several ways to create the association (for example, data bindings and code behind). The [MdispWPF](../../Examples/MdispWPF.md) example adds the following properties to the AILWPFDisplay tag to use the control in the code-behind and to bind the Aurora Imaging Library [`M_WPF`](../../Reference/disp/MdispAlloc.md) display (AilDisplayId) to the control.
   | Property | Sample Value | Notes |
   | --- | --- | --- |
   | ```<br/>x:Name<br/>``` | ```<br/>_ailWPFDisplayControl<br/>``` | This property is a standard identifier for all XAML objects see: [learn.microsoft.com/en-us/dotnet/desktop/xaml-services/xname-directive](http://learn.microsoft.com/en-us/dotnet/desktop/xaml-services/xname-directive). |
   | ```<br/>DisplayId<br/>``` | ```<br/>{Binding AilDisplayId}<br/>``` | This property is not required if you are using the code-behind method. For more on binding, see: [learn.microsoft.com/en-us/dotnet/desktop/wpf/data/?view=netdesktop-9.0](https://learn.microsoft.com/en-us/dotnet/desktop/wpf/data/?view=netdesktop-9.0). |
   For more information on dependency properties in WPF, see:[learn.microsoft.com/en-us/dotnet/desktop/wpf/advanced/dependency-properties-overview?view=netframeworkdesktop-4.8](https://learn.microsoft.com/en-us/dotnet/desktop/wpf/advanced/dependency-properties-overview?view=netframeworkdesktop-4.8).

> **Note:** For a complete example, see [MdispWPF](../../Examples/MdispWPF.md) in the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.

## XAML skeleton example

This is a complete WPF XAML page that includes Aurora Imaging Library. This page will require that a display is allocated using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md) with [`M_WPF`](../../Reference/disp/MdispAlloc.md).

```
<Window x:Class="MdispWPF.MainWindow" 
         xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
         xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
         xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
         xmlns:ailwpf="clr-namespace:Zebra.AuroraImagingLibrary.WPF;assembly=Zebra.AuroraImagingLibrary.WPF">
         
          <ailwpf:AILWPFDisplay x:Name="_ailWPFDisplayControl" 
                                DisplayId="{Binding AilDisplayId}" />
         
</Window>
```
