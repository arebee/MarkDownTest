---
Source File Name: 40-Logging.docx
AssetID: 8b4b7563-0062-4690-bfc2-df37f15b2d35
Title: Configuring Formatters
Order In ToC: 1-4-1-2-4
Output Filename: 1-4-1-2-4_Configuring Formatters.markdown
---

#### Markdown Test ####
# Configuring Formatters #
----------

The first procedure describes how to configure the formatters. The text formatter converts a log entry into a text string. The contents of the string are determined by replacing tokens in the text formatter's **Template** property. The **BinaryLogFormatter** uses the .NET Framework **BinaryFormatter** to serialize and deserialize the log entry in binary format. The **BinaryLogFormatter** is required when using the Message Queuing (MSMQ) trace listener with the Message Queuing distributor service. The **JsonLogFormatter** formats the log message using JavaScript Object Notation (JSON): this formatter is useful when you are logging to a text file.  
<a name="config_formatters" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>**To configure formatters**

1. Click on the plus sign icon in the **Log Message Formatters** section of the **Logging Settings **configuration pane and click **Add Log Message Formatters** to display the list of formatters. When you select a formatter, it is added to the **Log Message Formatters** section where you can then set its properties. 
2. Click the expander arrow in the formatter item you wish to configure if the properties are not visible.
3. (Optional) Change the **Name** property. The default name is the name of the formatter.
4. If you are adding a **text formatter** and want to change the template that contains the value placeholders, click the ellipsis button (**…**) for the **Template **property. Make the required changes in the **Template Editor** dialog. You can use the **Insert Token** button to add tokens with the appropriate syntax. After you finish making the changes, click **OK**. 
5. If you are adding a **Custom Formatter** and want to add or edit the settings, change the list of keys and values in the **Attributes** property section.
6. Repeat the procedure for each formatter you want to use.![](images/note.gif)Note:&gt; The only property you can change for the binary formatter is the name.


# Text Formatter Template Tokens #
The Template Editor dialog allows you to insert tokens into the template that the text formatter uses to format the text of a log entry. There are tokens for a wide range of values, including all of the values included by default in a log entry such as the message, priority, timestamp, severity, and category.  
There are also three special tokens named **property()**, **dictionary()**, and **keyvalue() **that you can use to access additional information included in the log entry, and tokens for a TAB character and a new line character combination. The default template created by the configuration tools shows examples of how you can use these function tokens. Note that property names are case-sensitive. For example, to include the Activity ID of a log entry in the message (it is not included in the default template), you must use the token **{property(ActivityId)}**.   
Timestamp tokens can include the **local: **prefix, which indicates that the timestamp should be displayed using the local time. Some examples of local timestamp format codes include **{timestamp(local)}**, which uses the default format string** **and **{timestamp(local:F)}**, which uses the **F **format string that represents the full date/time pattern. For more information about date/time formatting, see [Standard DateTime Format Strings](http://msdn2.microsoft.com/en-us/library/az4se3k1.aspx) on MSDN.  
Extracting some values from the environment, and formatting commonly used values such as the date and time, can have an effect on performance. To mitigate this, you can use specific tokens parameters that are directly mapped to high-speed formatting implementations. There are three date/time parameters that you can use with the **timestamp **token to maximize performance, and you can also use the **local:** prefix with these if required. The three high-speed implementations are:  
+ **timestamp(FixedFormatISOInternationalDate)**, which generates a date in the format <i>yyyy-MM-dd</i>. For a local date/time, use **timestamp(local:FixedFormatISOInternationalDate)**.
+ **timestamp(FixedFormatUSDate)**, which generates a date in the format <i>MM/dd/yyyy</i>. For a local date/time, use** timestamp(local:FixedFormatUSDate)**.
+ **timestamp(FixedFormatTime)**, which generates a time in the format <i>HH:mm:ss.fff</i>. For a local date/time, use** timestamp(local:FixedFormatTime)**.
There are four high-speed tokens for local context information. These tokens do not extract values from the log entry; instead, they use cached values. The four token are:  
+ **localAppDomain**, which inserts the name of the current application domain.
+ **localMachine**, which inserts the name of the local computer.
+ **localProcessName**, which inserts the name of the current process.
+ **localProcessId**, which inserts the ID of the current process.

# The JSON Formatter #
The **JsonLogFormatter** uses the JSON format to write individual log messages. This is useful if you require structured log messages in a text-based log file. The JSON formatter has a single configuration option called **JsonFormatting** that controls the layout of the log message. This configuration option has two possible values:  
+ **None**. No special formatting is applied. This is the default.
+ **Indented**. Causes child objects to be indented.

