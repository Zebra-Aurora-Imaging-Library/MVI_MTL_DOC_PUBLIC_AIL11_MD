---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Adding_and_deleting_string_models_to_the_context
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Adding and deleting string models to the context"
---

# Adding and deleting string models to the context

Once you have added a font to the font list in the String Reader context, you must add at least one string model to the string model list in that context. By default, the string model list is empty.

Each string model in the string model list represents a template, a way of locating a string in the target image. The settings in this template (string model) have been configured with defaults that are typically sufficient for many applications. It is recommended that you try the defaults first. The following sections in this chapter describe the various string model settings and how to change them. For example, you can set the minimum and maximum number of characters in the string, the maximum number of strings to locate (for an individual string model and for the String Reader context), the acceptance level for the string, and the valid range of scale. You can also set various types of character constraints, which can be used with all fonts in the String Reader context, or can be restricted to a specific font.

After you have adjusted all your string model settings, you must preprocess the context before calling [`MstrRead`](../../Reference/str/MstrRead.md). Note that a string in the target image will only be read if it satisfies every string model criteria, and if it respects the minimum definition of a string (that is, a linear sequence of regular characters).

## Adding and deleting the string models

To add a string model to the string model list in the String Reader context, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_ADD`](../../Reference/str/MstrControl.md). The maximum number of string models that you can add to a String Reader context is 255. The read operation time can increase with the number of string models in the context.

When a string model has been added to the context, it is assigned an index. String models are indexed as positive, consecutive integers starting at zero, and increasing by one. You can use the index to access a string model.

To control a string model setting, you must use [`MstrControl`](../../Reference/str/MstrControl.md) with the [`M_STRING_INDEX()`](../../Reference/str/MstrControl.md) macro set to the index of a specific string model, or to all string models.

You can also delete a string model or all string models from the string model list. To do so, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_DELETE`](../../Reference/str/MstrControl.md) set to the index of a specific string model, or to all string models ([`M_ALL`](../../Reference/str/MstrControl.md)). Note that when a string model is deleted, its index is reassigned; that is, index values greater than that of the removed string model are reduced by one.

To return the index of the string model read, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_STRING_MODEL_INDEX`](../../Reference/str/MstrGetResult.md).

String models can also be given a user-defined numeric label, using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_USER_LABEL`](../../Reference/str/MstrControl.md). A user label can be used as a means of identifying your string model, independently from its index in the String Reader context. However, user labels cannot be used as a direct replacement for the index; to retrieve the index of a string model from the user label, use [`MstrInquire`](../../Reference/str/MstrInquire.md) with [`M_STRING_INDEX_FROM_LABEL`](../../Reference/str/MstrInquire.md). The user label, once converted to an index value, can be used with any String Reader function that takes an index value. All user labels must be unique integers; that is, no two user labels can have the same integer. To remove a label from a string model, set the user label to [`M_NO_LABEL`](../../Reference/str/MstrControl.md).

## Preprocess and read

After you add string models and fonts to your String Reader context and before reading a string from the target image, the String Reader context must be preprocessed, otherwise an error will occur. It is during this preprocessing stage that String Reader prepares the context for the read operation by executing a number of preread calculations. To preprocess the String Reader context, use [`MstrPreprocess`](../../Reference/str/MstrPreprocess.md). To read a string from the target image, use [`MstrRead`](../../Reference/str/MstrRead.md).

Note that you might have to preprocess the String Reader context again if you change some of its control settings, such as the nominal angle of the string ([`M_STRING_ANGLE`](../../Reference/str/MstrControl.md)). To check if you need to preprocess the String Reader context, use [`MstrInquire`](../../Reference/str/MstrInquire.md) with [`M_PREPROCESSED`](../../Reference/str/MstrInquire.md).

### Saving and restoring

When you save a String Reader context, its preprocessing information is not saved. You must therefore preprocess the context when it is restored, even if you haven't changed any of its settings. To save a String Reader context to a file, use [`MstrSave`](../../Reference/str/MstrSave.md). To restore the context, use [`MstrRestore`](../../Reference/str/MstrRestore.md).

## Target image foreground

By default, strings are read assuming that the foreground is black. If it is white, or if it can be either white or black, you must specify this information in the string model, using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_FOREGROUND_VALUE`](../../Reference/str/MstrControl.md). Each string model can have a different foreground color. Note that only this string model setting affects what the read operation assumes is the foreground color; the foreground set when adding a user-defined font does not. For more information, see [Source image foreground](S04_Creating_and_customizing_the_fonts_for_a_font_based_context.md).
