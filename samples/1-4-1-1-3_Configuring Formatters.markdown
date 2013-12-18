---
Source File Name: 40-Logging.docx
AssetID: 6a496a7c-4637-4d3d-aa46-cb8ec1af8fca
Title: Configuring Formatters
Order In ToC: 1-4-1-1-3
Output Filename: 1-4-1-1-3_Configuring Formatters.markdown
---

#### Markdown Test ####
# Configuring Formatters #
----------

The Logging Application Block uses formatters to convert log entries into an appropriate format. The block includes the following formatter classes: **BinaryLogFormatter**, **JsonLogFormatter**, **TextFormatter**, and **XmlFormatter**. You configure the formatters by passing values to the constructor when you instantiate the formatter object.  
The **TextFormatter** class converts a log entry into a text string. The contents of the string are determined by replacing tokens in the text formatter's **Template** property. The **BinaryLogFormatter** class uses the .NET Framework **BinaryFormatter** class to serialize and deserialize the log entry in binary format. The **BinaryLogFormatter** class is required when using the Message Queuing (MSMQ) trace listener with the Message Queuing distributor service. The **JsonLogFormatter** class formats the log message using JavaScript Object Notation (JSON): this formatter is useful when you are logging to a text file. The **XmlFormatter** class formats the log message using XML: this formatter is useful when you are logging to a text file.  
The following sections describe each formatter type.  

# BinaryLogFormatter #
This formatter has no configuration options.  

# TextFormatter #
The **Template** property enables you to define a custom format for your log entries: it uses special tokens to represent specific values that you want to include in your formatted message. There are tokens for a wide range of values, including all of the values included by default in a log entry such as the message, priority, timestamp, severity, and category.  
The following code sample illustrates the use of the **TextFormatter** class:  

```csharp
TextFormatter formatter = new TextFormatter(
"Timestamp: {timestamp(local)}{newline}
Message: {message}{newline}
Category: {category}{newline}
Priority: {priority}{newline}
EventId: {eventid}{newline}
ActivityId: {property(ActivityId)}{newline}
Severity: {severity}{newline}
Title:{title}{newline}");
```

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
The following table lists all of the available tokens:  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Token name</p></th><th><p>Description</p></th></tr><tr><td><p>newline</p></td><td><p>A new line character.</p></td></tr><tr><td><p>tab</p></td><td><p>A tab character.</p></td></tr><tr><td><p>localMachine</p></td><td><p>The name of the local machine.</p></td></tr><tr><td><p>localProcessName</p></td><td><p>The name of the local process.</p></td></tr><tr><td><p>localAppDomain</p></td><td><p>The name of the local application domain.</p></td></tr><tr><td><p>localProcessId</p></td><td><p>The local process ID.</p></td></tr><tr><td><p>message</p></td><td><p>The message.</p></td></tr><tr><td><p>category</p></td><td><p>The trace source category.</p></td></tr><tr><td><p>priority</p></td><td><p>The message priority.</p></td></tr><tr><td><p>eventid</p></td><td><p>The event ID associated with the message.</p></td></tr><tr><td><p>severity</p></td><td><p>The severity of the message.</p></td></tr><tr><td><p>title</p></td><td><p>The message title.</p></td></tr><tr><td><p>errorMessages</p></td><td><p>Any associated error messages.</p></td></tr><tr><td><p>machine</p></td><td><p>The name of the machine.</p></td></tr><tr><td><p>appDomain</p></td><td><p>The name of the application domain.</p></td></tr><tr><td><p>processId</p></td><td><p>The process ID.</p></td></tr><tr><td><p>processName</p></td><td><p>The process name.</p></td></tr><tr><td><p>threadName</p></td><td><p>The thread name.</p></td></tr><tr><td><p>win32ThreadId</p></td><td><p>The thread ID.</p></td></tr><tr><td><p>activity</p></td><td><p>The activity associated with the message.</p></td></tr><tr><td><p>timestamp()</p></td><td><p>A timestamp.</p></td></tr><tr><td><p>property()</p></td><td><p>Access any property of the log entry.</p></td></tr><tr><td><p>keyvalue()</p></td><td><p>Access an entry in the <b>ExtendedProperties</b> property of the log entry.</p></td></tr><tr><td><p>dictionary()</p></td><td><p>A dictionary of all entries in the <b>ExtendedProperties</b> property of the log entry.</p></td></tr></table>

# JsonLogFormatter #
The **JsonLogFormatter** class uses the JSON format to write individual log messages. This is useful if you require structured log messages in a text-based log file. The constructor has a single optional configuration option called formatting of type **JsonFormatting** that controls the layout of the log message. The **JsonFormatting** enumeration has two possible values:  
+ **None**. No special formatting is applied. This is the default.
+ **Indented**. Causes child objects to be indented.

# XmlLogFormatter #
The **XmlFormatter** class uses the XML format to write individual log messages. This is useful if you require structured log messages in a text-based log file. There are no configuration options for this formatter.  
