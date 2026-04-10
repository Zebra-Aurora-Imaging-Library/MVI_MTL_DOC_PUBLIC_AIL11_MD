---
doctype: UserGuide
part: "Miscellaneous"
chapter: About_the_Help
section: Help_tips_online_and_Linux
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / About_the_Help / Help tips online and Linux"
---

# Tips for using the Help

The Help is accessible from Aurora Imaging Control Center or online. You can also access the Help from Visual Studio if you have installed [Aurora Imaging Library's Visual Studio add-on](../C02_Building_an_application/S25_Using_Addon_to_Visual_Studio.md).

## Help view settings

To view or change your Help settings, click the *[Image: Gear_Button_New_Help.png]*

 button and adjust the options in the **Help view settings** dialog. Modifying these settings is optional.

Customized settings narrow or broaden the scope of information presented in a Reference page such as [`MsysAlloc`](../../Reference/sys/MsysAlloc.md).

You can specify the following **Development environment information**:

|   |   |
| --- | --- |
| **Aurora Imaging Library systems** | Choose either **All** or **Specific systems**. For **Specific systems**, select the hardware product(s) or system(s) for which you are developing your application. |
| **Product** | Choose either **Aurora Imaging Library** or **Aurora Imaging Library Lite**. When you select the Lite version, unavailable functions will be listed in red on a module's information page. |
| **Computer** and **Operating system** | Choose **All** or the computer/operating system with which your are working. |

You can also specify the following **Reference table view options**:

|   |   |
| --- | --- |
| **Descriptions** | Set the default view to **Expanded** or **Collapsed**. When in **Collapsed** view, values tables in the Reference will display top-level table entries with a one-line description. You can individually expand any entry as required to see the full description and lower-level table entries. |
| **Systems information** | Set the default view to **Expanded** or **Collapsed**. **Expanded** view shows systems information in columns on the right side of relevant tables. When in **Collapsed** view, hover your mouse over the **‡** icon for a list of applicable systems. |

### Availability

The *[Image: reference_availability.png]*

 icon appears next to the **Availability** button when navigating to a User Guide or Reference page relevant to your settings. Click on **Availability** to display whether the current topic is applicable to Aurora Imaging Library, Aurora Imaging Library Lite, and to which of the supported hardware and operating systems.

## Navigation and display options

The following table shows the different ways to navigate the Help or limit the information displayed:

| Feature | Location | Description |
| --- | --- | --- |
| **Contents tab** | On the left | Offers a hierarchical view of the Help, letting you jump to specific pages. Right-clicking a contents item opens a context menu for collapsing folders and locating the current topic. |
| **Index tab** | On the left | Allows navigation to a specific constant's location(s) in the Reference, or to a specific function's reference page. You can enter a term in the edit field, scroll through and click on an item, or use your arrow keys. |
| [Search tab](S03_Help_tips_online_and_Linux.md) | On the left | Allows you to have specialized searches through the Help for more specific information. |
| **Filters button** | In the menu bar | Allows for filtering of Reference information. Typically, long reference pages provide a *[Image: filters_icon.png]*<br/><br/>**Filters** button. Click **Filters** to view and apply filters, when available. |
| **Page map** | On the right | Offers a hierarchical view of the current page while showing your current location. Click on a topic to jump to that subsection. To hide/display the page map, click the**Page map** button in the menu bar. |
| **INQ/SET/INFO buttons** | In the Reference | Allow you to navigate to the function and constant to use to inquire or set a specific setting. Hover on *[Image: INQ_icon_New_Help.png]*<br/><br/> or *[Image: SET_icon_New_Help.png]*<br/><br/> to see the relevant information. Clicking on the link in the boxed note will bring you to the referenced location. In some cases, the *[Image: INFO_icon_New_Help.png]*<br/><br/> icon offers a link to more information. |
| **Tooltips** | Hover over items | Offer quick information, typically for sidebar items, such as those listed in the **Contents** or **Page map**. Of note in the Reference, you can hover over: - The **‡** icon for systems information (when in collapsed view on relevant pages).<br/>- Empty columns on the left in parameter association tables to display the independent parameter value that corresponds to the current dependent parameter value. *[Image: New_Help_Parameter_Association.png]* |

To limit the displayed information in the Reference:

||
||
|  |
| **Collapse/Expand tables button** | Narrows/broadens information in values tables. You can use the arrows at the top of a table and at each individual value to collapse/expand just that specific table or value. You can change the default setting from the **Help view settings** dialog (*[Image: Gear_Button_New_Help.png]*<br/><br/>button); choose to expand/collapse **Descriptions**. |

To limit the displayed information in the User Guide and hardware-specific notes:

|   |   |
| --- | --- |
| **Collapse/Expand sections button** | Hides/shows subsections of the current section in the User Guide. You can use the arrows next to the subsection titles to collapse/expand just that subsection. |

To limit margin content:

|   |   |
| --- | --- |
| **Resize margins ** | Resize the columns on either side of any page by dragging the vertical border. To hide the **Contents**, **Index**, and **Search** tabs, double-click on the vertical border. |

### Search tab

To navigate the **Search** tab:

|  |
||
|  |
| **Simple mode** | Enter a search term in the text box provided. The search supports the following operators: |
| **"** | Matches the exact phrase enclosed in double quotation marks. |
| ***** | Matches 0 or more characters or symbols (for example,  matches [`M_COLOR`](../../Reference/gra/MgraControl.md) and [`M_BACKGROUND_COLOR`](../../Reference/disp/MdispControl.md)). To have the search match a space, enable the **Allow * wildcard to cross words** option. Note that when you disable **Match similar words** and use a wildcard search (with *), you can add another * at the end of your search term to find additional characters at the end of the term. For example, the query  will not find [`M_3D_DISPARITY_BASELINE`](../../Reference/buf/MbufControl.md), whereas  will. |
| **?** | Matches one character, space, or symbol. |
| **Options** | Allow you to narrow the scope of your search query. Click **Options** to see a customizable list; hover your mouse over an option to see a brief description. Note that **Match similar words** is enabled by default. To limit which part of the Help is searched, you can expand **Search locations** and enable the required part. > **Note:** Once you customize the search options, the **Reset** button will have a blue exclamation mark *[Image: availability_icon.png]*<br/>><br/>>  to its left, to remind you that you are no longer searching with default options. Click **Reset** to return search settings to their default state. |
| **Popup search dialog** | Allows you to navigate to the results that were actually found. Things to note: - This is especially useful if you have performed a wildcard (*) search.<br/>- If there are too many hits on the page, you can search for the required word by performing an additional search with **Search previous results** enabled.<br/>- To disable highlighting, click on the slider at the bottom left of the dialog.<br/>- You can always use your browser's search functionality to search your page. |
| **Regular expression mode** | See your browser's regular expression (regex) JavaScript syntax. You can set up searches using different groupings such as `\b[a-c]\w*`, which matches any word that begins with an a, b, or c. For more information and examples, you can visit [regex101.com](https://www.regex101.com). When entering your search expression, don't write it between slashes. By default, the Help performs global, case-insensitive searches (`/---/gi`). The regex search flags are not available; the `g` (global) and `i` (case-insensitive) options are performed internally. To match case, enable the **Match case** option in the **Search** tab (this internally removes the `i` regex flag). |
| **Max # highlighted instances per term** | Sets the maximum number of results to highlight on a page per term. Decreasing this number increases the load speed of a page accessed through the search results. |

## Sample code

Features of the Sample code button:

|   |   |
| --- | --- |
| **Sample code button** | Allows you to jump to calls to the current function in [Examples](../../Examples/index.md) that are included in the Help. If there are many examples, click on**Search for other examples**. This searches the examples included in the Help for a call to the function. Note that**Search location** is automatically set to Sample Code. It is recommended to click on **Reset** or adjust your search locations after searching through the examples. |

## Printing Aurora Imaging Library Help

To print the page you are viewing, right-click on it and select **Print**.

If printing from the Reference, some system columns might not print due to the width of the selected paper size. It is recommended that you print pages from the Reference using the **Landscape** option. You should also enable background printing.

It can also help to hide the page map before printing. To do so, click on **Page map** in the menu bar.
