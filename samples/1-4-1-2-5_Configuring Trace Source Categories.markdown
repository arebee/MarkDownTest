---
Source File Name: 40-Logging.docx
AssetID: 9301547d-44c4-490c-91a0-b63e86e4b6a2
Title: Configuring Trace Source Categories
Order In ToC: 1-4-1-2-5
Output Filename: 1-4-1-2-5_Configuring Trace Source Categories.markdown
---

#### Markdown Test ####
# Configuring Trace Source Categories #
----------

Trace source categories associate log entries with their target listener(s). You can configure the following two category types:  
+ **Categories** that specify the logging categories you want to use. When you create a log entry, you can assign it to one or more categories. Each category can filter the log entry based on its severity (such as **Critical**, **Error**, **Warning**, or **Information**), and route it to one or more trace listeners (logging targets).
+ **Special Categories **that automatically receive all log entries, unprocessed log entries, or log entries where an error occurred during logging.  Each of these special categories can filter the log entry based on its severity (such as **Critical**, **Error**, **Warning**, or **Information**), and route it to one or more trace listeners (logging targets).

# Configuring Trace Source Categories #
The next procedure describes how to assign log entries to categories by using the configuration tool.   
<a name="config_categorysources" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>**To configure Categories**

1. Add a category by clicking the plus sign icon in the **Categories** pane and clicking **Add Category**. A new category is added to the configuration with its properties displayed. If a category's properties are not visible, click the expander arrow for the category you want to configure. 
2. (Optional) In the properties pane, edit the name of the category. 
3. Click the **Minimum Severity** property and select the severity level of the events that will be assigned to this category. For information about the source level values, see the tables that follow this procedure. 
4. Click the **Auto Flush** property and select **True** if you want the listener to flush its data to the target after each new entry is written to this source. Select **False** if you want to flush the entries in your code only when required.
5. To add a logging target listener, click the **Listeners** property's plus icon and select a listener from the drop-down list.
6. To remove a listener from the list, click the cross icon. You can also make a new selection from the drop-down list for an existing listener to change the contents of the list.
7. Repeat the procedure for each category you require.

# Configuring Trace Sources Special Category #
The next procedure describes how to configure the special categories. The **Logging Errors &amp; Warnings** special category receives log entries for errors and warnings that occur during the logging process. The **Unprocessed Category** special category receives all log entries that are not processed by a source category (such as entries that specify a category that is not configured). The **All Events** special category receives all log entries.  
<a name="config_specialtraces" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>**To configure Special Categories**

1. In the **Special Categories** section, click the special source category you want to configure. 
2. (Optional) In the properties pane, type the name of the category. 
3.  (Optional) Set the **Minimum Severity** property. For information about the **SourceLevels** property values, see tables that follow this procedure.
4. Click the **Auto Flush** property and select **True** if you want the listener to flush its data to the target after each new entry is written to this source. Select **False** if you want to flush the entries in your code only when required.
5. To add a logging target listener, click the **Listeners** property's plus icon and select a listener from the drop-down list.
6. To remove a listener from the list, click the delete(x) icon. You can also make a new selection from the drop-down list for an existing listener to change the contents of the list.
7. Repeat the procedure for each special category source you wish to configure.
![](images/note.gif)Note:&gt; If an error occurs when the block writes to a special trace source, there is no error reported and the original log message is discarded.
# Source Category Properties #
<a name="props_categorysources" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following table lists the **Category Source** properties.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p><b>Property</b></p></th><th><p><b>Description</b></p></th></tr><tr><td><p><b>Name</b></p></td><td><p>The name of the category. The default is <b>Category Source</b>.</p></td></tr><tr><td><p><b>Minimum Severity</b></p></td><td><p>Possible values are <b>ActivityTracing</b>, <b>All</b>, <b>Critical</b>, <b>Error</b>, <b>Information</b>, <b>Off</b>, <b>Verbose</b>, and <b>Warning</b>. The default is <b>All</b>. See the following table for more information. Represented by the <b>filter </b>element in an XML file. See <hlink xlink:type="simple" xlink:show="new" xlink:actuate="onRequest" xlink:href="9301547d-44c4-490c-91a0-b63e86e4b6a2.html#switchlevel">SourceLevels</hlink>.</p></td></tr><tr><td><p><b>Auto Flush</b></p></td><td><p>Specifies whether the listener will flush its data to the target after every new entry is written to this source. The default value is <b>True</b>. When set to <b>False</b>, code can flush the entries when required. Note that it is your responsibility to ensure that all entries are flushed to the target, especially if an exception or failure occurs in the application. Otherwise, you will lose any cached logging information not yet written to the target.</p></td></tr><tr><td><p><b>Listeners</b></p></td><td><p>The name of the trace listeners that receive log entries and write them to the appropriate destinations.</p></td></tr></table>
# Severity (SourceLevels) Values #
<a name="switchlevel" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following table lists the possible values for the severity property of a category or a trace listener.   
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p><b>Value</b></p></th><th><p><b>Description</b></p></th></tr><tr><td><p><b>ActivityTracing</b></p></td><td><p>Allows the <b>Stop</b>, <b>Start</b>, <b>Suspend</b>, <b>Transfer</b>, and <b>Resume</b> events through. </p></td></tr><tr><td><p><b>All</b></p></td><td><p>Allows all events through.</p></td></tr><tr><td><p><b>Critical</b></p></td><td><p>Allows only <b>Critical</b> events through. A critical event is a fatal error or application crash.</p></td></tr><tr><td><p><b>Error</b></p></td><td><p>Allows <b>Critical</b> and <b>Error</b> events through. An <b>Error</b> event is a recoverable error.</p></td></tr><tr><td><p><b>Information</b></p></td><td><p>Allows <b>Critical</b>, <b>Error</b>, <b>Warning</b>, and <b>Information</b> events through. An information event is an informational message.</p></td></tr><tr><td><p><b>Off</b></p></td><td><p>Does not allow any events through.</p></td></tr><tr><td><p><b>Verbose</b></p></td><td><p>Allows <b>Critical</b>, <b>Error</b>, <b>Warning</b>, <b>Information</b>, and <b>Verbose</b> events through. A <b>Verbose</b> event is a debugging trace.</p></td></tr><tr><td><p><b>Warning</b></p></td><td><p>Allows <b>Critical</b>, <b>Error</b>, and <b>Warning</b> events through. A <b>Warning</b> event can indicate both critical and non-critical issues.</p></td></tr></table>
