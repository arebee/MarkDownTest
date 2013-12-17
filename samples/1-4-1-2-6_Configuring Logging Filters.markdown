---
Source File Name: 40-Logging.docx
AssetID: ac913544-cc72-4de9-b916-f9d85d473685
Title: Configuring Logging Filters
Order In ToC: 1-4-1-2-6
Output Filename: 1-4-1-2-6_Configuring Logging Filters.markdown
---

#### Markdown Test ####
# Configuring Logging Filters #
----------

You can filter log entries based on their categories and their priorities. You can also entirely enable or disable logging or add a custom logging filter. The following procedure describes how to configure the logging filters.  
<a name="config_filters" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>**To configure logging filters **

1. Select **Add Logging Filters** as described in <a href="test-markdown_3a6ba613-78b1-4b24-b226-55e368e41554.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Configuration Overview</a>. 
2. If the filter's property pane is not displayed, click the property expander arrow or right-click the filters** **node you wish to configure.
3. The **Category Filter** allows or denies log entries based on their categories:+ If you want to use a Category Filter, you can select the specific category filter by editing the **Categories** property. Click the plus sign icon to add a category filter. 
+ Type or select the category name for the category that will be filtered. You can either select a name from the drop-down list or type a category name. All Category Filters that have been added to the **Category Filters** section are available for selection in the drop-down list. The selected category name appears in the lower pane. 
+ Repeat to add any other categories you require.
+ To remove a category, click the delete (x) icon beside the category you wish to remove. 
+ Repeat to remove any other categories you no longer require.
+ Select a filter mode, either **AllowAllExceptDenied** or **DenyAllExceptDenied**.
+  (Optional) Change the name of the category filter. For information about the **Category** **Filter** properties, see <a href="#filter_category" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Category Filter Properties</a>.

4. The **Logging Enabled Filter** provides a global switch that you can use to turn logging on and off:+ If you want to log events, set **All Logging Enabled** to **True**. To prevent all logging, set **All Logging Enabled** to **False**.
+ (Optional) Change the name of the **Logging Enabled Filter**. For information about the **Logging Enabled Filter** properties, see <a href="#filter_logenabled" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Logging Enabled Filter Properties</a>.

5. The **Priority Filter** allows or denies log entries based on their priority:+ (Optional) Set the **Maximum Priority** property. This is the maximum priority value a log entry can have in order to be logged. If you do not set this property, the value is 2147483647 (this is the largest possible value of a 32-bit signed integer). 
+ (Optional) Set the **Minimum Priority** property. This is the minimum value a log entry must have to be logged. If you do not set this property, the value is **-1**.
+ (Optional) Change the name of the priority filter. For information about the priority filter properties, see <a href="#filter_priority" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Priority Filter Properties</a>.

6. A **Custom Logging Filter** is a class that you have created that derives from the **LogFilter** class. Its configuration information consists of a collection of name/value string pairs:+ Set the **Type** property by clicking the ellipsis button (...) to display the Type Selector. Navigate to and click the filter type name in the **Type Selector** dialog box. You must select a class that derives from the **LogFilter** class. It must also have the type **CustomLogFilterData** specified as the value of the **ConfigurationElementType** attribute placed on the class.
+ Enter a key/value pair in the **Key** and **Value** text boxes. When you enter a key/value pair, a new pair of blank text boxes are displayed. 
+  (Optional) Change the name of the **Custom Logging Filter**. For information about the **Custom Logging Filter** properties, see <a href="#filter_custom" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Custom Logging Filter Properties</a>.

7. Repeat the preceding steps for each filter you require. 

# Category Filter Properties #
<a name="filter_category" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following table lists the properties that appear when you add a **Category Filter** to the **Logging Filters **section.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p><b>Property</b></p></th><th><p><b>Description</b></p></th></tr><tr><td><p><b>Categories</b></p></td><td><p>Specifies specific categories that are explicitly allowed, or specific categories that are denied log entries. Use the <b>Category Filter Editor</b> to configure the expression.</p></td></tr><tr><td><p><b>Filter Mode</b></p></td><td><p>The <b>CategoryFilterMode</b> property that specifies if the filter will allow only messages that match one of the configured categories to pass to the logging target (<b>DenyAllExceptAllowed</b>), or will allow all message except those that match one of the configured categories to pass to the logging target (<b>AllowAllExceptDenied</b>).</p></td></tr><tr><td><p><b>Name</b></p></td><td><p>The name of the filter. The default is <b>Category Filter</b>.</p></td></tr></table>
# Logging Enabled Filter Properties #
<a name="filter_logenabled" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following table lists the properties that appear when you add a **Logging Enabled Filter**.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p><b>Property</b></p></th><th><p><b>Description</b></p></th></tr><tr><td><p><b>All Logging Enabled</b></p></td><td><p>The filter is enabled when this property value is <b>True</b>. The default is <b>False</b>.</p></td></tr><tr><td><p><b>Name</b></p></td><td><p>The name of the filter. The default is <b>Logging Enabled Filter</b>.</p></td></tr></table>
# Priority Filter Properties #
<a name="filter_priority" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following table lists the properties that appear when you add a **Priority Filter**.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p><b>Property</b></p></th><th><p><b>Description</b></p></th></tr><tr><td><p><b>Maximum Priority</b></p></td><td><p>The maximum value a log entry can have to be logged. If you do not set this property, there is no maximum value.</p></td></tr><tr><td><p><b>Minimum Priority</b></p></td><td><p>The minimum value a log entry must have to be logged. If you do not set this property, there is no minimum value. The default is <b>-1</b>.</p></td></tr><tr><td><p><b>Name</b></p></td><td><p>The name of the filter. The default is <b>Priority Filter</b>.</p></td></tr></table>
# Custom Logging Filter Properties #
<a name="filter_custom" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following table lists the properties that appear when you add a **Custom Logging Filter**.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p><b>Property</b></p></th><th><p><b>Description</b></p></th></tr><tr><td><p><b>Attributes</b></p></td><td><p>The key/value pairs for this filter.</p></td></tr><tr><td><p><b>Name</b></p></td><td><p>The name of the filter. The default is <b>Custom Logging Filter</b>.</p></td></tr><tr><td><p><b>Type</b></p></td><td><p>The type of the trace listener. Select one from the drop-down list. This is required.</p></td></tr></table><a name="loggingblock" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>
